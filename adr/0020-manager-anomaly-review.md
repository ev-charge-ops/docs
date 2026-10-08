# ADR 0020 — Revisão das anomalias pelo gestor

- **Status:** aceita
- **Data:** 2026-10-08
- **Complementa:** ADR 0012 (detecção de anomalias)

## Contexto

A detecção de anomalias (ADR 0012) grava um score de 0 a 1 em cada sessão encerrada e sinaliza as que passam de 0,5. O ADR 0012 deixou um risco em aberto: a decisão do gestor sobre cada alerta não era registrada. Sem esse registro, o portal não distinguia uma anomalia já analisada de uma nova, e não havia rótulos para recalibrar o limiar ou treinar um modelo supervisionado no futuro.

Também precisávamos deixar claro que a IA não muda a cobrança sozinha. O modelo é treinado com dados públicos, avaliado com anomalias sintéticas e pode errar.

## Decisão

- **Fila de revisão na própria sessão:** novos campos `anomalyReviewStatus` (`PENDING_REVIEW`, `CONFIRMED`, `DISMISSED` ou `null` quando a sessão não foi sinalizada), `anomalyReviewNote`, `anomalyReviewedAt` e `anomalyReviewedById`.
  - Ao gravar o score, uma sessão com `isAnomaly = true` entra como `PENDING_REVIEW` ([`anomaly-review.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/domain/anomaly-review.ts)).
  - A migração marcou como `PENDING_REVIEW` as sessões que já estavam sinalizadas em produção.
- **Decisão do gestor:** `POST /organizations/:organizationId/sessions/:sessionId/anomaly-review` ([`review-session-anomaly`](https://github.com/ev-charge-ops/api/tree/main/src/modules/charging-sessions/commands/review-session-anomaly)), só para `MANAGER` da organização:
  - corpo `{ status: CONFIRMED | DISMISSED, note? }`, com nota de até 500 caracteres;
  - só aceita sessões sinalizadas (`409 SESSION_NOT_FLAGGED` nas outras). Uma nova revisão substitui a anterior;
  - a gravação usa o controle otimista de concorrência da sessão, com até 3 tentativas.
- **A cobrança não muda:** confirmar ou descartar não altera energia, multa, captura no cartão nem o rateio. A revisão é um registro de auditoria, e o sistema não tem ajuste de valor nesta versão.
- **Onde aparece:**
  - na visão geral, `anomaliesPendingReviewCount` e a lista de anomalias recentes com o botão "Revisar";
  - na lista de sessões, o filtro `reviewStatus` e o selo "Confirmada" ou "Descartada" com o score;
  - na gaveta da sessão, o cartão do score com os sinais avaliados, a seção "Revisão do gestor" e os botões "Confirmar anomalia" e "Descartar sinalização";
  - as respostas do motorista não mostram o status da revisão.

## Consequências

- **Positivas:**
  - A IA estrutural ganha um ciclo fechado: o modelo sinaliza, o gestor decide e a decisão fica registrada com autor, data e observação.
  - O portal deixa claro que nenhuma cobrança muda sem revisão do gestor, o que reduz o risco de um falso positivo virar conflito entre condôminos.
  - As decisões acumuladas viram rótulos reais para recalibrar o limiar de 0,5 e, mais adiante, treinar um modelo supervisionado.
- **Negativas e riscos:**
  - Como a revisão não muda valores, uma anomalia confirmada ainda exige ajuste manual da cobrança.
  - O histórico de revisões não é versionado: uma nova decisão substitui a anterior.
  - Os rótulos ainda não alimentam o treino. O retreino com eles é um próximo passo.
  - A API devolve só o score, não a contribuição de cada variável, então o portal mostra os sinais avaliados, e não um ranking de fatores.
- **Desvio da Sprint 01:** o plano tratava a anomalia como alerta para o gestor. Agora o alerta tem um fluxo de revisão com decisão registrada, que não estava no plano.
