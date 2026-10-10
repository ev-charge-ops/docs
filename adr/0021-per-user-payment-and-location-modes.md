# ADR 0021 — Modos de pagamento e de localização por usuário

- **Status:** aceita
- **Data:** 2026-10-08
- **Complementa:** ADR 0014 (pré-autorização no Stripe)

## Contexto

O app foi para a revisão da App Store. A equipe da Apple precisa de uma conta que funcione de ponta a ponta, mas o carregador é simulado e todas as contas usavam o Stripe em modo de teste e o centro fixo do condomínio de demonstração no mapa:

- **Pagamento:** com chaves de teste, um cartão real é recusado. O revisor precisa pagar de verdade no ponto comercial, sem que o valor fique cobrado.
- **Localização:** o revisor está longe da Aclimação. Com o centro fixo, o mapa abriria em São Paulo e pareceria quebrado.
- **Demais contas:** as contas de demonstração e as dos moradores precisam continuar exatamente como estavam, em modo de teste e com o centro do condomínio.

Até aqui não havia nenhuma configuração individual no usuário, e o gateway de pagamento era único, escolhido pela `STRIPE_SECRET_KEY`.

## Decisão

- **Três campos no `User`**, expostos em `GET /auth/me` e no `user` de todas as respostas de sessão ([api#38](https://github.com/ev-charge-ops/api/pull/38)):
  - `paymentMode` (`TEST` | `LIVE`, padrão `TEST`): o modo do Stripe usado nos pagamentos do usuário;
  - `locationMode` (`DEMO` | `DEVICE`, padrão `DEMO`): de onde o app tira a posição;
  - `autoRefund` (padrão `false`): estorno integral logo depois da captura.
- **Dois gateways do Stripe** ([api#39](https://github.com/ev-charge-ops/api/pull/39)):
  - o `TEST`, com as variáveis atuais, e o `LIVE`, com `STRIPE_LIVE_SECRET_KEY`, `STRIPE_LIVE_WEBHOOK_SECRET` e `STRIPE_LIVE_PUBLISHABLE_KEY`;
  - o modo é decidido no início da sessão comercial e gravado em `Payment.mode`. Captura, cancelamento, PaymentSheet e estorno usam sempre o gateway gravado;
  - um cliente Stripe por modo (`stripeCustomerId` e `stripeLiveCustomerId`) e um webhook por modo (`/payments/stripe/webhook` e `/payments/stripe/webhook/live`), cada um reconciliando só as sessões do seu modo;
  - um usuário `LIVE` sem as chaves de produção recebe `503 PAYMENTS_UNAVAILABLE`; os usuários `TEST` não são afetados.
- **Estorno automático:** quando a captura de um pagamento com `autoRefund` dá certo, o acerto da sessão cria um estorno integral com chave de idempotência. O pagamento passa ao status `REFUNDED`, com `refundedCents` e `refundedAt`, e o recibo do app mostra "Estornado". Uma falha do Stripe é tentada de novo na próxima leitura da sessão.
- **Localização por usuário no app** ([mobile#61](https://github.com/ev-charge-ops/mobile/pull/61)): com `DEVICE`, o app pede a permissão e centraliza o mapa no GPS. Se não houver ponto num raio de 50 km, aparece "Nenhum ponto perto de você", com um atalho para o ponto mais próximo ou para o mapa do Brasil.
- **Conta da revisão no seed** (`SEED_REVIEWER_EMAIL` e `SEED_REVIEWER_PASSWORD`): motorista com e-mail verificado, morador do Residencial Aclimação (unidade "Revisão · 01"), `LIVE`, `DEVICE` e `autoRefund`. Sem a senha, o seed pula a conta. A senha nunca entra no repositório; ela vai só para o campo da App Store Connect.

## Consequências

- **Positivas:**
  - A revisão vê o fluxo real de pagamento e o mapa na própria cidade, sem mudar nada para a banca e para os moradores.
  - O estorno automático evita cobrança por uma recarga que não existiu, mantendo o Stripe em produção auditável.
  - Os campos servem para outras contas no futuro, como um piloto pago com GPS real.
- **Negativas:**
  - Duas configurações do Stripe para manter (chaves, webhooks e clientes).
  - A conta `LIVE` gera taxas do Stripe em cada captura, mesmo com o estorno.
  - Mudar o modo de um usuário só vale para as próximas sessões; as abertas continuam no modo gravado.
