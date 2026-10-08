# Evidências — Sprint 02

Lista do que a equipe precisa capturar para a entrega. Salve cada arquivo nesta pasta com o nome indicado e marque o item quando ele estiver no repositório. Use PNG para as capturas e prefira as telas com os dados do seed (Residencial Aclimação).

As telas seguem o visual Pulse (app 1.3.0 e portal atual). Para conferir se a captura está completa, compare com a prancheta da mesma tela no [canvas do Pulse](https://claude.ai/artifact/BWq3Na6AKkLLsnc3KUbHgR). Capturas do visual antigo, escuro com vermelho, não servem.

## Vídeo

- [ ] **Vídeo da demonstração** (até ~5 min, publicado no YouTube como "não listado"). Depois de publicar, coloque o link no [README principal](../README.md#10-evidências) e no `.TXT` da entrega. Roteiro sugerido:
  1. **Problema e arquitetura** (30 s): o problema do condomínio e o diagrama do README.
  2. **Portal, como gestor** (1 min 15 s):
     - Login.
     - Visão geral: pontos em uso, capacidade elétrica com o pico do mês, indicadores contra o mês anterior e anomalias.
     - Revisão de uma anomalia: confirmar ou descartar com observação, mostrando que o valor não muda.
     - Sessões com "Somente anomalias" e a gaveta de detalhes.
     - Rateio mensal com a exportação do CSV.
     - Passada rápida por Pontos e capacidade, Regras de tarifa (com a simulação) e Moradores.
  3. **App, recarga num ponto privado** (1 min 45 s):
     - Início com o condomínio e os pontos.
     - Pontos: mapa noturno e lista por distância.
     - Detalhe do L1-01 com o preço repassado e o fator informativo.
     - Iniciar recarga com limite de 80%.
     - Recarga ao vivo: carregando, tolerância com a contagem regressiva, lembrete local e ocupação com multa.
     - Encerrar e mostrar o recibo.
     - Histórico com o extrato do mês da unidade B · 42.
  4. **De volta ao portal** (20 s): a sessão nova em Sessões, com o score, e no rateio da unidade B · 42.
  5. **Ponto de visitantes** (40 s): recarga no L2-01 com a tela de pagamento (tarifa base × fator), o PaymentSheet com o cartão de teste `4242 4242 4242 4242` e, no painel do Stripe (modo de teste), a pré-autorização e a captura.
  6. **IA e fallback** (30 s): `GET /health` do `ml`, uma chamada a `/demand-factor` e o fallback por regras (`demandFactorSource = RULE`) quando o `ml` não responde.

## Capturas de tela

### Portal do gestor (app.evchargeops.com.br)

- [ ] `portal-01-login.png`: Login, com o painel de vídeo, e-mail e senha, Google, Apple e "Receber link de acesso por e-mail"
- [ ] `portal-02-visao-geral.png`: Visão geral com o cartão "Agora", a capacidade elétrica, os 4 indicadores, a energia por semana e as anomalias detectadas
- [ ] `portal-03-revisao-anomalia.png`: gaveta de revisão de uma anomalia, com "Confirmar anomalia" e "Descartar sinalização"
- [ ] `portal-04-sessoes.png`: Sessões com os filtros, o interruptor "Somente anomalias" e a coluna de score
- [ ] `portal-05-sessao-detalhe.png`: gaveta de uma sessão sinalizada, com o cartão do score, Recarga, Cobrança e Revisão do gestor
- [ ] `portal-06-rateio.png`: Rateio mensal com o cartão "Total a ratear", a fórmula e a tabela por unidade
- [ ] `portal-07-rateio-csv.png`: CSV exportado aberto no Excel ou no Google Sheets
- [ ] `portal-08-pontos.png`: Pontos e capacidade, com a capacidade elétrica, o preço dinâmico (fonte `MODEL` ou `RULE`) e os cartões dos pontos com foto
- [ ] `portal-09-tarifa.png`: Regras de tarifa com a simulação de recarga de 15 kWh
- [ ] `portal-10-moradores.png`: Moradores com a tabela e os convites pendentes
- [ ] `portal-11-convite.png`: gaveta "Convidar morador" com a prévia do e-mail

### App do motorista (versão 1.3.0)

- [ ] `app-01-entrar.png`: tela de entrar
- [ ] `app-02-consentimento.png`: consentimentos LGPD no primeiro acesso, se aparecer
- [ ] `app-03-inicio.png`: Início com a recarga em andamento ou o card vazio, e os pontos do condomínio
- [ ] `app-04-pontos-mapa.png`: Pontos em modo mapa (noturno), com os pins e o card inferior
- [ ] `app-05-pontos-lista.png`: Pontos em modo lista, ordenado por distância
- [ ] `app-06-detalhe-privado.png`: detalhe do L1-01 sobre o mapa, com foto, preço repassado, fator informativo e regra do condomínio
- [ ] `app-07-iniciar.png`: sheet "Iniciar recarga" com o limite em % (e os modos kWh e R$)
- [ ] `app-08-recarga-ao-vivo.png`: recarga ao vivo (carregando), com o anel, kWh, potência, tempo e valor
- [ ] `app-09-tolerancia.png`: tolerância (`GRACE`), com a contagem "sem multa por"
- [ ] `app-10-ocupacao.png`: ocupação com multa (`IDLE`), com a barra até o teto
- [ ] `app-11-lembrete.png`: lembrete local ou push na tela de bloqueio ("Recarga concluída" ou "Tolerância acabando")
- [ ] `app-12-recibo.png`: recibo da sessão encerrada, com a quebra e a curva de potência
- [ ] `app-13-historico.png`: Histórico com o extrato do mês da unidade e a lista de recargas
- [ ] `app-14-conta.png`: Conta com o perfil, o resumo do mês e as opções de segurança e privacidade
- [ ] `app-15-privacidade.png`: Privacidade e dados, com as finalidades, exportação e exclusão
- [ ] `app-16-avisos.png`: Notificações abertas pelo sino
- [ ] `app-17-pagamento.png`: tela "Pagamento no cartão" do L2-01, com tarifa base × fator e a pré-autorização
- [ ] `app-18-paymentsheet.png`: PaymentSheet do Stripe com o cartão de teste
- [ ] `app-19-recibo-cartao.png`: recibo com o valor capturado no cartão
- [ ] `app-20-fila.png` (opcional): detalhe de um ponto ocupado com "Entrar na fila" ou o banner "Reservado para você"

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
- [ ] `infra-02-eas.png`: EAS Workflows com o build 1.3.0 ou a atualização OTA
- [ ] `infra-03-testflight.png`: build no TestFlight

## Cuidados

- Não capture senhas, chaves de API, segredos de webhook nem variáveis de ambiente.
- Recorte ou esconda e-mails pessoais. Os e-mails das contas de demonstração podem aparecer.
- Capture com "reduzir movimento" desligado no aparelho e no navegador, para os vídeos e as animações aparecerem como no canvas.
