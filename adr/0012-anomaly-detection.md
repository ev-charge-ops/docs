# ADR 0012 — Detecção de anomalias nas sessões

- **Status:** aceita
- **Data:** 2026-10-07
- **Complementada por:** ADR 0020 (revisão das anomalias pelo gestor)

## Contexto

O rateio por kWh só é justo se as medições forem confiáveis. Erros de medidor, energia impossível para o tempo conectado e carros que passam dias ocupando a vaga distorcem a conta do mês e geram conflito entre condôminos. A Sprint 01 listou o Isolation Forest para "sessões atípicas ou uso indevido".

Não existem rótulos reais de anomalia, nem nos dados públicos nem no condomínio, então o modelo precisa ser não supervisionado.

## Decisão

- **Modelo:** `IsolationForest` (scikit-learn), com um modelo por tipo de ponto (`PRIVATE` e `COMMERCIAL`), treinado só com sessões válidas dos conjuntos públicos da Noruega (residencial) e de Turku, na Finlândia (público), ambos CC BY 4.0.
  - Variáveis: energia, duração, tempo ocioso, potência média, potência durante a carga e a hora de início em seno e cosseno.
  - O score bruto vira um score de 0 a 1 por tipo de ponto: 0,5 corresponde ao percentil 98 do treino, e `isAnomaly = score ≥ 0,5`.
  - O treino e a avaliação estão no [notebook 03](https://github.com/ev-charge-ops/ml/blob/main/notebooks/03-anomaly-detection.ipynb) e no [model card](https://github.com/ev-charge-ops/ml/blob/main/artifacts/anomaly/v1/model-card.md).
- **Avaliação:** com 400 anomalias sintéticas injetadas em 7.956 sessões normais de teste, o modelo teve F1 de 0,72, ROC-AUC de 0,97 e 2,1% de sessões normais sinalizadas. As 309 sessões reais com mais de 72 h foram todas sinalizadas.
- **Quando pontuar:** ao encerrar a sessão (`POST /sessions/:id/stop`), a API monta as variáveis com [`sessionFeatures`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/commands/stop-session/session-features.ts) e chama `POST /anomaly-score` pelo [`MlAnomalyScorer`](https://github.com/ev-charge-ops/api/blob/main/src/modules/intelligence/anomaly/ml-anomaly-scorer.ts). O resultado é gravado na sessão (`anomalyScore`, `isAnomaly`, `anomalyModelVersion`). O tempo usa a escala da simulação (ADR 0010), então a sessão acelerada é avaliada em minutos simulados.
- **Falha do serviço:** se o `ml` não responder em 1,5 s ou devolver algo inválido, a sessão fecha normalmente e fica sem score (`null`). Sem `ML_URL`, o [`DisabledAnomalyScorer`](https://github.com/ev-charge-ops/api/blob/main/src/modules/intelligence/anomaly/anomaly-scorer.port.ts) é usado. O encerramento e a cobrança nunca esperam pelo modelo.
- **Uso no portal:** a anomalia é um alerta para o gestor revisar, nunca um bloqueio.
  - A visão geral lista as anomalias recentes.
  - A lista de sessões filtra por anomalia (`?anomaly=true`).
  - O detalhe da sessão explica o score com as variáveis usadas.
  - A sessão sinalizada continua no rateio. Cabe ao gestor decidir o que fazer.
- **Seed:** além do histórico normal, o seed cria 8 sessões anômalas (pico de medidor, rajada de energia, ocupação de vários dias e ociosidade extrema), pontuadas pelo serviço `ml` em produção. Se ele não estiver disponível, uma regra simples entra no lugar ([`demo-anomalies.ts`](https://github.com/ev-charge-ops/api/blob/main/prisma/demo-anomalies.ts)).

## Consequências

- **Positivas:**
  - É a segunda aplicação estrutural da IA: ela fica no fechamento de toda sessão, e não numa análise separada.
  - O gestor tem uma lista curta e ordenada para revisar antes de fechar o mês.
- **Negativas e riscos:**
  - As anomalias da avaliação são sintéticas, e a precisão real depende da frequência de anomalias na operação.
  - A ocupação "fantasma" (horas conectado quase sem carregar) é pouco detectada em `PRIVATE` (cobertura de 12%), porque é comum em garagens residenciais. Essa situação é tratada pela multa por ocupação.
  - Sessões `INTERRUPTED` não são pontuadas.
  - Ainda não registramos a decisão do gestor sobre cada alerta. Esse registro geraria rótulos para recalibrar o limiar. **Atualização (2026-10-08):** resolvido pelo ADR 0020, em que o gestor confirma ou descarta cada sinalização sem alterar a cobrança.
- **Desvio da Sprint 01:** o plano colocava a detecção de anomalias na Fase 4. Ela entrou como planejado, e o modelo de precificação (ADR 0011) também, de modo que o MVP tem dois modelos em produção, além do mínimo de um previsto nos critérios de sucesso.
