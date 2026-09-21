# ADR 0014 — Pré-autorização no Stripe para o ponto comercial

- **Status:** aceita
- **Data:** 2026-10-07

## Contexto

O ponto de visitantes do condomínio é `COMMERCIAL`: quem carrega não é condômino e não entra no rateio mensal. A cobrança precisa ser garantida antes de liberar a energia, mas o valor final só é conhecido no encerramento, porque depende dos kWh e da eventual multa por ocupação. A Sprint 01 definiu o Stripe com pré-autorização no início e captura no encerramento, e estabeleceu que os dados do cartão nunca passam pela nossa infraestrutura.

## Decisão

- **PaymentIntent com captura manual** ([`StripePaymentGateway`](https://github.com/ev-charge-ops/api/blob/main/src/modules/payments/adapters/stripe-payment.adapter.ts)): `capture_method: manual`, só cartão, em BRL. O `Customer` do Stripe é criado uma vez por usuário. Todas as chamadas usam chaves de idempotência.
- **Valor bloqueado** ([`holdAmountCents`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/domain/session-payment.ts)): energia máxima (o limite em kWh do motorista ou `PAYMENT_HOLD_ENERGY_KWH`, 60 kWh por padrão) × tarifa travada, limitado ao valor em R$ quando o motorista escolhe esse limite, mais o teto da multa por ocupação. O mínimo é R$ 0,50.
- **Fluxo:**
  1. `POST /sessions` num ponto comercial cria a sessão em `AWAITING_PAYMENT` e devolve os parâmetros do PaymentSheet (client secret, customer e ephemeral key).
  2. O app abre o PaymentSheet do `@stripe/stripe-react-native`. O cartão é digitado no componente do Stripe e não passa pela API.
  3. O webhook `POST /payments/stripe/webhook` (assinatura verificada, eventos `payment_intent.*`, idempotente pela tabela `ProcessedPaymentEvent`) ou o `POST /sessions/:id/payment/confirm` chamado pelo app reconciliam a sessão. Com o intent em `requires_capture`, a sessão vai para `PENDING` e o carregador é acionado.
  4. No encerramento, a API captura `min(total da sessão, valor autorizado)`. Se o total ficar abaixo do mínimo, ou se a sessão for interrompida antes de carregar, o bloqueio é cancelado.
- **Teto de energia:** a meta de energia da sessão é limitada ao que cabe no valor autorizado, descontado o teto da multa. Assim, a captura nunca precisa passar do bloqueio.
- **Prazo:** se a autorização não chegar em `PAYMENT_AUTHORIZATION_TIMEOUT_MINUTES` (15 min reais por padrão), a sessão é interrompida e o intent é cancelado.
- **Desligável:** a integração fica atrás da port [`PaymentGateway`](https://github.com/ev-charge-ops/api/blob/main/src/modules/payments/payment-gateway.port.ts). Sem `STRIPE_SECRET_KEY`, iniciar sessão em ponto comercial responde `503 PAYMENTS_UNAVAILABLE` e os pontos privados continuam funcionando.
- **Modo de teste:** produção usa as chaves de teste do Stripe. Na demonstração, o cartão é `4242 4242 4242 4242`, com qualquer validade futura e qualquer CVC.

## Consequências

- **Positivas:**
  - Não há inadimplência no ponto de visitantes, e o motorista só paga o que consumiu.
  - Os dados do cartão ficam só no Stripe, o que reduz o escopo de PCI e de LGPD.
  - Webhook e confirmação pelo app levam ao mesmo estado, o que torna o fluxo tolerante a atraso ou perda de evento.
- **Negativas e riscos:**
  - Pré-autorizações de cartão expiram em cerca de 7 dias. Uma sessão aberta por mais tempo que isso não seria capturada.
  - Sem NFS-e: a emissão fiscal da rede comercial não foi implementada.
  - Por estar em modo de teste, não há cobrança real.
- **Desvio da Sprint 01:** a pré-autorização foi aplicada só ao ponto comercial. Na rede privada a cobrança é pelo rateio mensal (ADR 0013), o que evita bloquear saldo de condôminos a cada recarga. A emissão automática de NFS-e e a taxa percentual da plataforma sobre a transação ficaram fora do MVP.
