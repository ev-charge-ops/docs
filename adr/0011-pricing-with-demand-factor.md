# ADR 0011 — Precificação com fator de demanda da IA e fallback por regras

- **Status:** aceita
- **Data:** 2026-10-07

## Contexto

A Sprint 01 definiu a cobrança por kWh com tarifa ajustada pela IA (`tarifa_kWh = tarifa_base × fator_demanda`). Ela também registrou uma restrição regulatória: na rede privada (condomínio) não pode haver margem sobre a energia, que é repassada a custo (ANEEL RN 1.000/2021). No ponto comercial (visitantes), o preço é livre.

A IA roda num serviço separado (`ml`). O preço precisa sair mesmo que esse serviço esteja lento ou fora do ar, porque a recarga não pode ser bloqueada por causa do modelo.

## Decisão

- **Tarifa versionada:** a tabela `Tariff` guarda `utilityRateCents` (tarifa da concessionária), `baseRateCents` (base do ponto comercial), `accessFeeCents` (taxa de acesso mensal), `idleFeeCentsPerMinute`, `idleFeeCapCents` e `gracePeriodMinutes`.
  - Cada alteração feita pelo gestor cria uma versão nova com `validFrom`, e a vigente é a mais recente.
  - Uma tarifa específica do ponto tem prioridade sobre a da organização ([`tariff-rules.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charge-points/tariff-rules.ts)).
  - Valores do seed: R$ 0,89/kWh da concessionária, R$ 1,89/kWh de base comercial, R$ 35,00 de taxa de acesso, R$ 0,25/min de multa com teto de R$ 30,00 e 10 min de tolerância.
- **Preço por kWh** (`pricePerKwhCents`):
  - ponto `PRIVATE`: `tarifa da concessionária`. O fator é calculado e exibido ao morador só como sinal informativo (`demandFactorApplied = false`);
  - ponto `COMMERCIAL`: `round(tarifa base × fator de demanda)`.
- **Multa por ocupação:** depois da tolerância, `min(teto, minutos ociosos × valor por minuto)` ([`session-fees.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/domain/session-fees.ts)).
- **Total da sessão:** `kWh × tarifa travada + multa por ocupação`. A tarifa é travada no início da sessão (ADR 0010).
- **Fator de demanda pelo modelo** ([`MlDemandFactorProvider`](https://github.com/ev-charge-ops/api/blob/main/src/modules/intelligence/demand-factor/ml-demand-factor.provider.ts)):
  - a API chama `POST /demand-factor` do serviço `ml` com a hora e o dia da semana no fuso de São Paulo, a ocupação do local (pontos ocupados ÷ pontos online da organização), o tamanho da fila e o tipo do ponto;
  - o modelo `GradientBoostingRegressor` prevê o índice de demanda das próximas 3 h, que é convertido em fator por uma regra linear por partes (0,25 → 0,8; 0,50 → 1,0; 0,90 → 1,5), sempre limitado a [0,8; 1,5]. Os detalhes estão no [model card](https://github.com/ev-charge-ops/ml/blob/main/artifacts/demand_factor/v1/model-card.md);
  - a API só aceita fatores finitos entre 0,5 e 3, arredonda para 2 casas e classifica a faixa: `OFF_PEAK` abaixo de 0,95, `PEAK` acima de 1,2 e `NORMAL` no meio.
- **Fallback por regras** ([`RuleDemandFactorProvider`](https://github.com/ev-charge-ops/api/blob/main/src/modules/intelligence/demand-factor/rule-demand-factor.provider.ts)):
  - pico (1,5): das 18 h às 21 h, ou ocupação ≥ 80%, ou fila;
  - fora de pico (0,8): das 22 h às 6 h com ocupação < 50%;
  - normal (1,0) no restante.

  O fallback é usado quando `ML_URL` está vazio, quando a chamada passa de 1,5 s, quando a resposta não é 2xx ou quando o fator é inválido.
- **Rastreabilidade:** cada sessão grava `demandFactor`, `demandFactorSource` (`MODEL` ou `RULE`) e `demandModelVersion`. O portal e o app mostram a origem do fator.
- **Contrato:** a integração fica atrás da port [`DemandFactorProvider`](https://github.com/ev-charge-ops/api/blob/main/src/modules/intelligence/demand-factor/demand-factor.provider.ts). O [`IntelligenceModule`](https://github.com/ev-charge-ops/api/blob/main/src/modules/intelligence/intelligence.module.ts) monta o provider do modelo com as regras como fallback.

## Consequências

- **Positivas:**
  - A IA está no caminho do preço de verdade, e não num relatório à parte.
  - A recarga nunca depende do serviço `ml` para começar. No pior caso, o atraso é de 1,5 s.
  - Respeita a restrição da rede privada: o condômino paga o custo da energia, e o fator só orienta o horário.
- **Negativas e riscos:**
  - A fila (`queueLength`) ainda é sempre 0, porque não existe reserva nem fila no produto. O modelo aceita a variável, mas hoje ela não pesa.
  - O modelo foi treinado com dados da Noruega e da Finlândia. Os pesos e a curva de conversão precisam ser recalibrados com o histórico real do condomínio.
  - O fator é calculado para o local inteiro (ocupação da organização), não por ponto.
- **Desvio da Sprint 01:** a fórmula foi mantida. A taxa de acesso mensal entrou no rateio (ADR 0013), mas não na sessão. Previsão de custo por regressão, clustering K-Means e previsão de capacidade por séries temporais, listados como abordagens de IA, ficaram fora do MVP. O app mostra o preço por kWh do momento antes de iniciar.
