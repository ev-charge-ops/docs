# ADR 0017 — Notificações push e lembretes projetados

- **Status:** aceita
- **Data:** 2026-10-08
- **Complementa:** ADR 0010 (máquina de estados e avanço sob demanda)

## Contexto

A pesquisa da Sprint 01 apontou a vaga ocupada por carro já carregado como o maior problema dos motoristas, e o plano previa avisos em três momentos: perto do fim, na conclusão e no início da multa. Para a tolerância de 10 minutos funcionar, o motorista precisa saber que a carga terminou mesmo com o app fechado.

Dois limites pesaram:

- a API é serverless e não tem processo em segundo plano. A sessão só avança quando alguém a lê (ADR 0010), então os eventos "recarga concluída" e "multa iniciada" só seriam detectados com o app aberto;
- no aparelho, os lembretes locais dependiam de `graceEndsAt` e `idleStartsAt`, que só existem depois que a recarga termina.

## Decisão

- **Caixa de avisos por usuário** (módulo [`notifications`](https://github.com/ev-charge-ops/api/tree/main/src/modules/notifications)):
  - `GET /me/notifications` com paginação e `unreadCount`, `POST /me/notifications/:id/read` e `POST /me/notifications/read-all`;
  - tipos: `SESSION_ACTIVE`, `CHARGING_COMPLETE`, `IDLE_FEE_STARTED`, `PAYMENT_CAPTURED`, `PAYMENT_FAILED`, `SESSION_INTERRUPTED`, `ORGANIZATION_INVITE` e `QUEUE_TURN` (ADR 0019), com títulos e textos em pt-BR;
  - como as transições acontecem na leitura, a criação é idempotente: cada aviso tem um `dedupeKey` único por sessão e tipo, gravado com `ON CONFLICT DO NOTHING`. Leituras repetidas não duplicam avisos.
- **Push pelo Expo:**
  - o app registra o token em `POST /me/push-tokens` depois do login e do aceite dos consentimentos (ADR 0018), e o remove em `DELETE /me/push-tokens/:token` no logout;
  - a porta `PushSender` tem dois adapters: `ExpoPushSender`, que envia em lotes de 100 com timeout de 5 s, e `ConsolePushSender`, que só registra no log. A variável `PUSH_DRIVER` (`console` ou `expo`) escolhe o adapter;
  - o ticket `DeviceNotRegistered` apaga o token. Os recibos de entrega não são consultados, porque não há processo em segundo plano para isso;
  - nenhuma falha de push ou de gravação do aviso quebra a requisição principal. Tudo vira `warn` no log.
- **Linha do tempo projetada:** o simulador do carregador é determinístico, então o fim da recarga pode ser calculado assim que a sessão fica `ACTIVE`.
  - A port `ChargerGateway` ganhou `projectCompletion`. O `MockChargerGateway` roda a mesma simulação até completar, inclusive a redução de potência acima de 80%. O adapter SEMS devolve `null`, porque com o carregador real o fim não é previsível.
  - As respostas de sessão trazem `projectedChargingEndsAt`, `projectedGraceEndsAt`, `projectedIdleStartsAt` e `projectedIdleFeeCapReachedAt`, em horário real (já com a simulação acelerada). Em `ACTIVE` vêm da projeção; em `GRACE` e `IDLE` repetem os horários reais; nos outros estados são `null` ([`session-projector.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charging-sessions/session-projector.ts)).
- **Lembretes locais no app** ([`session-reminders.ts`](https://github.com/ev-charge-ops/mobile/blob/main/src/features/charging/session-reminders.ts)):
  - três lembretes por sessão, com identificador fixo: "Recarga concluída" no fim projetado, "Tolerância acabando" 2 min antes do fim da tolerância (ou na metade, quando ela é curta, caso da simulação acelerada) e "Taxa de ocupação iniciada" no início da multa;
  - são agendados logo depois de iniciar a sessão, ainda com o app aberto, e reagendados só quando a projeção muda mais de 30 s. São cancelados quando a sessão fecha e no logout;
  - no Android, o canal tem importância alta, para o lembrete aparecer como heads-up.
- **Sem alerta duplicado:** quando a API detecta `CHARGING_COMPLETE` ou `IDLE_FEE_STARTED` mais de 60 s depois da transição, o aviso continua gravado na caixa, mas o push não sai, porque o aparelho já mostrou o lembrete local. Com o app aberto, o lembrete local e o push do mesmo alerta aparecem uma vez só.
- **Navegação:** tocar no aviso marca como lido e abre a sessão, o ponto (`QUEUE_TURN`) ou a conta (convite). No relayout Pulse (ADR 0016), os avisos saíram da tab bar e ficam no sino do cabeçalho, com o contador de não lidos.

## Consequências

- **Positivas:**
  - O motorista é avisado da conclusão e do início da multa mesmo com o app fechado, sem job agendado na API.
  - A caixa de avisos guarda o histórico mesmo quando o push não é entregue.
  - Como a projeção usa a mesma simulação da telemetria, o lembrete coincide com a transição real (há teste comparando os dois).
- **Negativas e riscos:**
  - A projeção depende de um carregador previsível. Com o adapter SEMS ela é `null`, e os avisos voltam a depender da leitura da sessão ou de um job.
  - Sem `PUSH_DRIVER=expo` no ambiente, os pushes só aparecem no log. Os avisos continuam na caixa do app.
  - Os recibos do Expo não são consultados, então um push perdido não é reenviado.
- **Desvio da Sprint 01:** o plano previa push em −15 min, na conclusão e no início da multa. Entraram a conclusão, o fim da tolerância (2 min antes) e o início da multa, mais os avisos de pagamento, interrupção, convite e fila. O aviso de −15 min não entrou.
