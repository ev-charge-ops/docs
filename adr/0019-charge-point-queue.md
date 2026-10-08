# ADR 0019 — Fila nos pontos de recarga com reserva de 10 minutos

- **Status:** aceita
- **Data:** 2026-10-08
- **Complementa:** ADR 0010 (sessão) e ADR 0011 (fator de demanda)

## Contexto

Com poucos carregadores para muitos motoristas, quem encontra o ponto ocupado não tem como saber quando ele vai liberar nem garantir a vez. Sem uma fila, quem passa primeiro na garagem pega o ponto, e o carro que esperava perde a vez. Além disso, o fator de demanda (ADR 0011) já previa o tamanho da fila como variável, mas ela era sempre 0 porque o produto não tinha fila.

A API não tem processo em segundo plano (ADR 0010), então a fila precisa andar sem job agendado.

## Decisão

- **Uma fila por ponto** ([`charge-points/queue`](https://github.com/ev-charge-ops/api/tree/main/src/modules/charge-points/queue)):
  - `POST /charge-points/:id/queue` entra na fila, `DELETE /charge-points/:id/queue` sai e `GET /charge-points/:id/queue/me` mostra a posição;
  - só dá para entrar na fila de um ponto ocupado e visível ao usuário. Os erros são explícitos: `CHARGE_POINT_AVAILABLE` (o ponto está livre, basta iniciar), `CHARGE_POINT_OFFLINE`, `QUEUE_OWN_SESSION`, `ALREADY_IN_QUEUE` e `ACTIVE_QUEUE_EXISTS`;
  - cada usuário fica em uma fila ativa por vez, garantido por lock na linha do usuário;
  - a lista e o detalhe dos pontos trazem `queueLength`, `reservedUntil` e `myQueueEntry`.
- **Estados da entrada:** `WAITING → NOTIFIED → FULFILLED`, ou `EXPIRED` e `LEFT`.
- **Reserva:** quando a sessão no ponto é encerrada ou interrompida, o primeiro da fila passa a `NOTIFIED`, recebe **10 minutos reais** de reserva e o aviso `QUEUE_TURN` ("Sua vez no ponto X"), com push (ADR 0017). Durante a reserva, só ele inicia a sessão; os demais recebem `409 CHARGE_POINT_RESERVED`.
- **Avanço na leitura:** como as sessões, a reserva vencida é avaliada em cada leitura do ponto, da fila e no início de sessão. A entrada vira `EXPIRED` e a vez passa ao próximo, com novo aviso ([`queue-rules.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/charge-points/queue/queue-rules.ts)).
- **Saída automática:** ao iniciar a sessão no ponto reservado, a entrada vira `FULFILLED`. Se o usuário carregar em outro ponto, a entrada vira `LEFT` e a fila daquele ponto anda.
- **Fator de demanda:** o `queueLength` real (soma das filas dos pontos do mesmo tipo no local) passa a ser enviado ao modelo e à regra de fallback, no lugar do 0 fixo.
- **No app:** no detalhe de um ponto ocupado, o CTA vira "Entrar na fila" e mostra a posição. Na vez do usuário, um banner "Reservado para você" mostra a contagem regressiva e libera "Iniciar recarga". Para os outros, o CTA fica "Reservado para a fila". A lista e o mapa mostram "N na fila".
- **Falhas:** um erro ao avançar a fila ou ao notificar só gera log. O encerramento da sessão não depende da fila.

## Consequências

- **Positivas:**
  - O motorista sabe a posição e tem a vez garantida por 10 minutos, o que reduz a disputa pela vaga.
  - A fila alimenta o fator de demanda com um sinal real de procura.
  - Não precisa de job: a fila anda nas mesmas leituras que já avançam as sessões.
- **Negativas e riscos:**
  - A reserva usa minutos reais, não a escala da simulação. Na demonstração acelerada, a reserva dura bem mais que a recarga simulada.
  - Se ninguém consultar o ponto, a reserva vencida só é liberada na próxima leitura. O resultado é o mesmo, mas o aviso ao próximo da fila pode atrasar.
  - Não há reserva com horário marcado, só a fila do momento.
- **Desvio da Sprint 01:** a Sprint 01 citava a fila como entrada do fator de demanda (`fator_demanda = IA(ocupacao, fila, historico, horario)`) e a pesquisa apontou fila e vaga ocupada como o maior problema (9 em 10), mas o plano não detalhava como a fila funcionaria. Ela entrou como fila por ponto com reserva curta, junto com a multa por ocupação.
