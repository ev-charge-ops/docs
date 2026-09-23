# Evidências — Sprint 02

Lista do que a equipe precisa capturar para a entrega. Salve cada arquivo nesta pasta com o nome indicado e marque o item quando ele estiver no repositório. Use PNG para as capturas e prefira as telas com os dados do seed (Residencial Aclimação).

## Vídeo

- [ ] **Vídeo da demonstração** (até ~5 min, publicado no YouTube como "não listado"). Depois de publicar, coloque o link no [README principal](../README.md#10-evidências) e no `.TXT` da entrega. Roteiro sugerido:
  1. Apresentação rápida do problema e da arquitetura (diagrama do README).
  2. Portal: visão geral, sessões com filtro de anomalia e rateio com exportação do CSV.
  3. App: lista de pontos com preço e fator de demanda, início de recarga num ponto privado, sessão ao vivo passando por carregando, tolerância e ocupação com multa, encerramento e recibo.
  4. Portal: a sessão nova aparece em Sessões, com score de anomalia, e no rateio da unidade B · 42.
  5. App: recarga no ponto de visitantes (L2-01) com o cartão de teste `4242 4242 4242 4242`, mostrando a pré-autorização e a captura no painel do Stripe (modo de teste).
  6. Serviço de IA: `GET /health` e uma chamada a `/demand-factor` pelo Swagger ou pelo terminal, e o fallback por regras (`demandFactorSource = RULE`) quando o `ml` não responde.

## Capturas de tela

### Portal do gestor (app.evchargeops.com.br)

- [ ] `portal-01-login.png`: tela de login (e-mail e senha, Google, Apple e link por e-mail)
- [ ] `portal-02-visao-geral.png`: visão geral do mês com consumo, valores, capacidade elétrica e anomalias recentes
- [ ] `portal-03-sessoes.png`: lista de sessões com filtros
- [ ] `portal-04-sessao-anomala.png`: detalhe de uma sessão sinalizada, com a explicação do score
- [ ] `portal-05-rateio.png`: rateio mensal por unidade com os totais
- [ ] `portal-06-rateio-csv.png`: CSV exportado aberto no Excel ou no Google Sheets
- [ ] `portal-07-pontos-capacidade.png`: pontos com preço atual, fator de demanda e origem (`MODEL` ou `RULE`)
- [ ] `portal-08-regras.png`: regras de tarifa (concessionária, base comercial, taxa de acesso, tolerância e multa)
- [ ] `portal-09-moradores-convite.png`: moradores e envio de convite

### App do motorista

- [ ] `app-01-login.png`: tela de entrada
- [ ] `app-02-pontos.png`: lista de pontos com estado e preço
- [ ] `app-03-ponto-privado.png`: detalhe de L1-01 com o preço repassado e o fator informativo
- [ ] `app-04-iniciar-recarga.png`: escolha do limite (100%, kWh ou R$)
- [ ] `app-05-sessao-ativa.png`: sessão carregando (kWh, kW, % de carga e valor)
- [ ] `app-06-tolerancia.png`: sessão em tolerância (`GRACE`)
- [ ] `app-07-ocupacao-multa.png`: sessão em ocupação com multa (`IDLE`)
- [ ] `app-08-recibo.png`: recibo da sessão encerrada
- [ ] `app-09-historico.png`: histórico de sessões
- [ ] `app-10-ponto-comercial.png`: detalhe de L2-01 com tarifa base × fator
- [ ] `app-11-paymentsheet.png`: PaymentSheet do Stripe com o cartão de teste
- [ ] `app-12-recibo-cartao.png`: recibo com o valor capturado

### API, IA e infraestrutura

- [ ] `api-01-swagger.png`: Swagger em api.evchargeops.com.br/docs
- [ ] `ml-01-health.png`: `GET https://ml.evchargeops.com.br/health` com as versões dos modelos
- [ ] `ml-02-demand-factor.png`: resposta de `POST /demand-factor`
- [ ] `ml-03-anomaly-score.png`: resposta de `POST /anomaly-score`
- [ ] `ml-04-notebook-demanda.png`: gráfico do notebook 02 (modelo contra persistência)
- [ ] `ml-05-notebook-anomalias.png`: gráfico do notebook 03 (score e limiar)
- [ ] `stripe-01-pagamento.png`: painel do Stripe (modo de teste) com o PaymentIntent pré-autorizado e depois capturado
- [ ] `ci-01-actions.png`: execuções verdes do GitHub Actions nos repositórios
- [ ] `infra-01-vercel.png`: projetos na Vercel com deploy de produção e preview de PR
- [ ] `infra-02-eas.png`: EAS Workflows com o build ou a atualização OTA
- [ ] `infra-03-testflight.png`: build no TestFlight

## Cuidados

- Não capture senhas, chaves de API, segredos de webhook nem variáveis de ambiente.
- Recorte ou esconda e-mails pessoais. Os e-mails das contas de demonstração podem aparecer.
