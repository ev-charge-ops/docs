# ADR 0010 — Máquina de estados da sessão de recarga e simulação acelerada

- **Status:** aceita
- **Data:** 2026-10-07

## Contexto

A sessão de recarga é o centro do produto: dela saem o consumo (kWh), o custo, a multa por ocupação e o rateio do mês. Precisávamos de um ciclo de vida explícito, que cobrisse também o pagamento antecipado do ponto comercial, a tolerância depois da carga completa e as falhas do carregador.

Dois limites pesaram na decisão:

- a integração com o GoodWe HCA G2 pela API SEMS não entrou nesta sprint, então a telemetria precisa ser simulada;
- a API roda como função serverless na Vercel, sem processo em segundo plano para avançar sessões abertas.

## Decisão

- **Estados** ([`session-status.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/domain/session-status.ts)):

  ```mermaid
  stateDiagram-v2
      [*] --> AWAITING_PAYMENT: ponto COMMERCIAL
      [*] --> PENDING: ponto PRIVATE
      AWAITING_PAYMENT --> PENDING: cartão pré-autorizado
      AWAITING_PAYMENT --> INTERRUPTED: cancelado ou prazo expirado
      PENDING --> ACTIVE: carregador iniciou
      PENDING --> INTERRUPTED: carregador falhou
      ACTIVE --> GRACE: carga completa ou limite atingido
      GRACE --> IDLE: fim da tolerância
      ACTIVE --> CLOSED: motorista encerra
      GRACE --> CLOSED: motorista encerra
      IDLE --> CLOSED: motorista encerra
      CLOSED --> [*]
      INTERRUPTED --> [*]
  ```

  - `GRACE` é a tolerância gratuita (padrão de 10 min) depois que a carga termina.
  - `IDLE` é a ocupação cobrada: multa por minuto, com teto.
  - `INTERRUPTED` registra a energia parcial, se houver, e não é cobrada no ponto comercial.
- **Entidade de domínio:** as transições ficam na entidade [`ChargingSession`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/domain/charging-session.entity.ts), sem depender do Nest nem do Prisma. Uma transição inválida lança `InvalidSessionTransitionError`.
- **Regras fixadas no início:** a tarifa por kWh, o fator de demanda e sua origem, a tolerância e a multa são gravados na sessão quando ela começa (`lockedRateCents`). Mudar a tarifa depois não altera uma sessão em andamento.
- **Limite de recarga:** o motorista escolhe entre `FULL` (até 100%), `ENERGY` (kWh), `AMOUNT` (R$) ou `PERCENT` (carga alvo da bateria, acima da carga atual do veículo; incluído em 2026-10-08). A meta de energia é calculada em [`charging-limit.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/domain/charging-limit.ts).
- **Exclusividade:** um motorista só tem uma sessão aberta e um ponto só atende uma sessão por vez (`ACTIVE_SESSION_EXISTS` e `CHARGE_POINT_BUSY`).
- **Capacidade do local:** antes de iniciar, a potência é alocada a partir da demanda contratada menos a reserva das áreas comuns e as sessões em curso ([`site-capacity.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charge-points/site-capacity.ts)). Sem a potência mínima disponível, a sessão é recusada.
- **Avanço sob demanda:** não há fila nem cron. Toda leitura (`GET /sessions/active`, `GET /sessions/:id`) e o encerramento passam pelo [`SessionSynchronizer`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/session-synchronizer.ts), que lê a telemetria até o instante atual, avança o estado e recalcula os valores. A gravação usa controle otimista de concorrência (`version`).
- **Carregador atrás de uma port:** [`ChargerGateway`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charger-gateway/charger-gateway.port.ts) define `start`, `stop` e `readTelemetry`, e a variável `CHARGER_DRIVER` escolhe o adapter.
  - `mock` ([`MockChargerGateway`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charger-gateway/adapters/mock-charger.adapter.ts)): simula um veículo de 50 kWh chegando com 42%, gera amostras a cada 5 min simulados e reduz a potência acima de 80% de carga.
  - `sems`: reservado para a API SEMS da GoodWe. Hoje responde `501 Not Implemented`.
- **Simulação acelerada:** `SIMULATION_SPEED` (padrão 60) define quantos segundos simulados passam a cada segundo real. Com 60, uma recarga de 1 h termina em 1 min e a tolerância de 10 min dura 10 s. O fator vai para a sessão (`timeScale`), então o cálculo de tempo, tolerância e multa usa a mesma escala.

## Consequências

- **Positivas:**
  - O domínio é testado sem banco nem rede, e os estados aparecem iguais na API, no app e no portal.
  - A demonstração mostra o ciclo completo (carga, tolerância, multa e fechamento) em poucos minutos.
  - Trocar o simulador pelo carregador real é implementar um adapter, sem tocar no domínio.
- **Negativas e riscos:**
  - Como o estado só avança quando alguém consulta a sessão, uma sessão esquecida fica aberta no banco até a próxima leitura. Os valores continuam corretos, porque são calculados a partir dos horários.
  - Não há integração real com o HCA G2. A telemetria exibida é simulada.
- **Desvio da Sprint 01:** o plano previa a API SEMS (Remote Control e Real-time Monitoring) já nesta sprint. Ela ficou para depois, atrás da port, e um simulador ocupa o lugar do carregador. OCPP, citado na Sprint 01 como evolução, também não foi implementado.
