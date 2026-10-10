# Evidências — Sprint 02

Capturas do app, do portal, do site do produto, da publicação na App Store e dos serviços em produção. Todas mostram o visual Pulse, que pode ser comparado com as pranchetas do [canvas do Pulse](https://claude.ai/artifact/BWq3Na6AKkLLsnc3KUbHgR). As capturas do visual antigo, escuro com vermelho, ficam em [`historico/`](historico/) só para registrar a evolução.

- **App:** capturas reais de um iPhone, com o app 1.4.x e a conta de um morador do Residencial Aclimação.
- **Portal:** capturas de 2026-10-08 e 2026-10-09, com a conta de demonstração do gestor. Nas telas com nomes, o morador real foi trocado por "Morador".
- **Pendentes:** os itens ainda sem arquivo estão marcados com `[ ]` no [roteiro](#roteiro-de-capturas).

## Sumário

1. [App do motorista](#app-do-motorista)
2. [Portal do gestor](#portal-do-gestor)
3. [Site do produto](#site-do-produto)
4. [App Store](#app-store)
5. [API, IA e infraestrutura](#api-ia-e-infraestrutura)
6. [Visual antigo](#visual-antigo)
7. [Roteiro de capturas](#roteiro-de-capturas)

## App do motorista

| Início | Início com recarga em andamento | Pontos (mapa) | Detalhe do ponto |
|---|---|---|---|
| <img src="app/app-03-inicio.png" width="200" alt="Início do app sem recarga ativa"> | <img src="app/app-03b-inicio-recarga-em-andamento.png" width="200" alt="Início com a recarga em andamento"> | <img src="app/app-04-pontos-mapa.png" width="200" alt="Mapa noturno com os pontos"> | <img src="app/app-06-detalhe-privado.png" width="200" alt="Detalhe do ponto L1-02"> |

| Iniciar recarga | Recarga ao vivo | Recarga ao vivo, mais adiante | Recarga com o limite |
|---|---|---|---|
| <img src="app/app-07-iniciar.png" width="200" alt="Sheet Iniciar recarga com limite de 80%"> | <img src="app/app-08-recarga-ao-vivo.png" width="200" alt="Recarga ao vivo com 0,38 kWh"> | <img src="app/app-08b-recarga-ao-vivo.png" width="200" alt="Recarga ao vivo com 0,32 kWh"> | <img src="app/app-08c-recarga-ao-vivo-limite.png" width="200" alt="Recarga ao vivo com o limite marcado"> |

| Encerrar recarga | Recibo (condomínio) | Recibo (cartão) | Recibo (cartão, com compartilhar) |
|---|---|---|---|
| <img src="app/app-10-encerrar.png" width="200" alt="Confirmação para encerrar a recarga"> | <img src="app/app-12-recibo.png" width="200" alt="Recibo de uma recarga no condomínio"> | <img src="app/app-19-recibo-cartao.png" width="200" alt="Recibo de uma recarga paga no cartão"> | <img src="app/app-19b-recibo-cartao.png" width="200" alt="Recibo pago no cartão com o botão compartilhar"> |

| Histórico e rateio do mês | Conta |
|---|---|
| <img src="app/app-13-historico.png" width="200" alt="Histórico com o rateio da unidade"> | <img src="app/app-14-conta.png" width="200" alt="Conta com perfil e privacidade"> |

## Portal do gestor

**Visão geral**, com capacidade elétrica, indicadores e anomalias:

![Visão geral do portal](portal/portal-02-visao-geral.jpg)

**Revisão de uma anomalia**, com confirmar ou descartar sem mudar a cobrança:

![Gaveta de revisão de anomalia](portal/portal-03-revisao-anomalia.jpg)

**Sessões**, com os filtros e o score de anomalia:

![Lista de sessões](portal/portal-04-sessoes.jpg)

**Rateio mensal** por unidade:

![Rateio mensal](portal/portal-06-rateio.jpg)

**Pontos e capacidade**, com o preço dinâmico:

![Pontos e capacidade](portal/portal-08-pontos.jpg)

**Regras de tarifa**, com a simulação de uma recarga de 15 kWh:

![Regras de tarifa](portal/portal-09-tarifa.jpg)

**Moradores** e **convite** com a prévia do e-mail:

![Moradores](portal/portal-10-moradores.jpg)

![Convidar morador](portal/portal-11-convite.jpg)

## Site do produto

[www.evchargeops.com.br](https://www.evchargeops.com.br), servido pelo projeto do portal ([ADR 0024](../adr/0024-landing-page-on-the-portal-project.md)).

![Abertura do site](site/site-01-inicio.jpg)

![Seção do condomínio com os prints do portal](site/site-02-condominio.jpg)

| Celular: capítulo "Encontrar" | Celular: capítulo "Iniciar" |
|---|---|
| <img src="site/site-03-mobile-encontrar.png" width="240" alt="Site no iPhone, capítulo Encontrar"> | <img src="site/site-04-mobile-iniciar.png" width="240" alt="Site no iPhone, capítulo Iniciar"> |

Imagens de compartilhamento (Open Graph) do site e do portal:

| Site | Portal |
|---|---|
| ![Compartilhamento do site](site/site-05-compartilhamento-landing.jpg) | ![Compartilhamento do portal](site/site-06-compartilhamento-portal.jpg) |

## App Store

Capturas enviadas à loja, em 1206×2622 (iPhone de 6,3"), feitas a partir de telas reais do app ([ADR 0025](../adr/0025-app-store-release.md)):

| | | | |
|---|---|---|---|
| <img src="app-store/screenshot-01.png" width="180" alt="Captura 1 da loja"> | <img src="app-store/screenshot-02.png" width="180" alt="Captura 2 da loja"> | <img src="app-store/screenshot-03.png" width="180" alt="Captura 3 da loja"> | <img src="app-store/screenshot-04.png" width="180" alt="Captura 4 da loja"> |
| <img src="app-store/screenshot-05.png" width="180" alt="Captura 5 da loja"> | <img src="app-store/screenshot-06.png" width="180" alt="Captura 6 da loja"> | <img src="app-store/screenshot-07.png" width="180" alt="Captura 7 da loja"> | |

**Revisão:** a versão 1.4.1 aguardando revisão. O envio anterior foi removido porque usava um build de preview:

![Revisão no App Store Connect](app-store/asc-01-revisao.jpg)

**E-mails da Apple** recusando builds sem os textos de permissão de câmera e de movimento (ITMS-90683). Os textos foram adicionados no `app.json`:

| | |
|---|---|
| <img src="app-store/asc-02-email-itms-90683-a.png" width="260" alt="E-mail da Apple ITMS-90683"> | <img src="app-store/asc-03-email-itms-90683-b.png" width="260" alt="Outro e-mail da Apple ITMS-90683"> |

## API, IA e infraestrutura

**Swagger** em [api.evchargeops.com.br/docs](https://api.evchargeops.com.br/docs):

![Swagger da API](infra/api-01-swagger.jpg)

**Serviço de IA:** `GET https://ml.evchargeops.com.br/health` em 2026-10-09:

```json
{"status":"ok","models":{"demandFactor":"v1","anomaly":"v1"}}
```

**Organização no GitHub**, com a logo e os dados do produto:

![Organização ev-charge-ops no GitHub](infra/org-01-github.jpg)

## Visual antigo

Capturas da primeira versão, escura com vermelho, antes do design system Pulse ([ADR 0016](../adr/0016-pulse-design-system.md)). Não valem como evidência do produto atual.

| Entrar (app) | Pagamento (app) | Recarga (app) | Avisos (app) | Entrar (portal) |
|---|---|---|---|---|
| <img src="historico/app-entrar-visual-antigo.png" width="150" alt="Tela de entrar antiga"> | <img src="historico/app-pagamento-visual-antigo.png" width="150" alt="Pagamento antigo"> | <img src="historico/app-recarga-visual-antigo.png" width="150" alt="Recarga antiga"> | <img src="historico/app-avisos-visual-antigo.png" width="150" alt="Avisos antigos"> | <img src="historico/portal-entrar-visual-antigo.png" width="150" alt="Login antigo do portal"> |

## Roteiro de capturas

### Vídeo

- [ ] **Vídeo da demonstração** (até ~5 min, publicado no YouTube como "não listado"). Depois de publicar, coloque o link no [README principal](../README.md#10-evidências) e no `.TXT` da entrega. Roteiro sugerido:
  1. **Problema e arquitetura** (30 s): o problema do condomínio e o diagrama do README.
  2. **Site do produto** (20 s): [www.evchargeops.com.br](https://www.evchargeops.com.br), rolando até a seção do condomínio.
  3. **Portal, como gestor** (1 min 15 s): Visão geral, revisão de uma anomalia, Sessões com "Somente anomalias", Rateio com o CSV e uma passada por Pontos, Regras de tarifa e Moradores.
  4. **App, recarga num ponto privado** (1 min 45 s): Início, mapa e lista, detalhe do L1-01, recarga com limite de 80%, recarga ao vivo com tolerância e multa, recibo e Histórico.
  5. **De volta ao portal** (20 s): a sessão nova em Sessões e no rateio da unidade.
  6. **Ponto de visitantes** (40 s): pagamento no L2-01 com o cartão de teste `4242 4242 4242 4242` e a pré-autorização e a captura no painel do Stripe (modo de teste).
  7. **IA e fallback** (30 s): `GET /health` do `ml`, uma chamada a `/demand-factor` e o fallback por regras.

### Portal do gestor

- [ ] `portal-01-login`: tela de entrar no visual Pulse
- [x] `portal-02-visao-geral`
- [x] `portal-03-revisao-anomalia`
- [x] `portal-04-sessoes`
- [ ] `portal-05-sessao-detalhe`: gaveta de uma sessão sinalizada. A captura ficou de fora porque o tempo das sessões ainda aparece como `0h00` na simulação acelerada
- [x] `portal-06-rateio`
- [ ] `portal-07-rateio-csv`: CSV exportado aberto numa planilha
- [x] `portal-08-pontos`
- [x] `portal-09-tarifa`
- [x] `portal-10-moradores`
- [x] `portal-11-convite`

### App do motorista

- [ ] `app-01-entrar`: tela de entrar no visual Pulse
- [ ] `app-02-consentimento`: consentimentos LGPD no primeiro acesso
- [x] `app-03-inicio` e `app-03b-inicio-recarga-em-andamento`
- [x] `app-04-pontos-mapa`
- [ ] `app-05-pontos-lista`: modo lista ordenado por distância
- [x] `app-06-detalhe-privado`
- [x] `app-07-iniciar`
- [x] `app-08-recarga-ao-vivo` (e `08b`, `08c`)
- [ ] `app-09-tolerancia`: recarga em tolerância (âmbar)
- [x] `app-10-encerrar`
- [ ] `app-11-ocupacao` e `app-11b-lembrete`: ocupação com multa e lembrete na tela de bloqueio
- [x] `app-12-recibo`
- [x] `app-13-historico`
- [x] `app-14-conta`
- [ ] `app-15-privacidade`: Privacidade e dados com "Excluir conta"
- [ ] `app-16-avisos`: notificações abertas pelo sino
- [ ] `app-17-pagamento` e `app-18-paymentsheet`: pagamento no L2-01 e PaymentSheet do Stripe
- [x] `app-19-recibo-cartao` (e `19b`)
- [ ] `app-20-fila` (opcional): "Entrar na fila" ou "Reservado para você"

### Site, loja, API, IA e infraestrutura

- [x] `site-01` a `site-06`: site no computador e no celular e imagens de compartilhamento
- [x] `app-store/screenshot-01` a `07` e `asc-01` a `03`
- [x] `api-01-swagger`
- [x] `ml-01-health` (texto acima)
- [ ] `ml-02-demand-factor` e `ml-03-anomaly-score`: respostas de `POST /demand-factor` e `POST /anomaly-score`
- [ ] `ml-04-notebook-demanda` e `ml-05-notebook-anomalias`: gráficos dos notebooks 02 e 03
- [ ] `stripe-01-pagamento`: PaymentIntent pré-autorizado e capturado no painel do Stripe (modo de teste)
- [ ] `ci-01-actions`: execuções verdes do GitHub Actions
- [ ] `infra-01-vercel`, `infra-02-eas` e `infra-03-testflight`
- [x] `org-01-github`

## Cuidados

- Não capture senhas, chaves de API, segredos de webhook nem variáveis de ambiente.
- Recorte ou esconda e-mails pessoais. Os e-mails das contas de demonstração podem aparecer.
- Capture com "reduzir movimento" desligado no aparelho e no navegador, para os vídeos e as animações aparecerem como no canvas.
