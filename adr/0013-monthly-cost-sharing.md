# ADR 0013 — Rateio mensal por unidade

- **Status:** aceita
- **Data:** 2026-10-07

## Contexto

O objetivo do desafio é sair das sessões, passar pelo consumo e chegar ao rateio justo. No condomínio (rede privada), a energia dos carregadores entra na conta comum e precisa ser repassada a cada unidade sem margem. A Sprint 01 definiu duas linhas de cobrança: uma taxa de acesso mensal por motorista habilitado, que rateia a infraestrutura, e o consumo em kWh a custo, mais a multa por ocupação.

O síndico precisa de um extrato por unidade que possa lançar no boleto condominial.

## Decisão

- **Sessões que entram no rateio** ([`findBillableSessions`](https://github.com/ev-charge-ops/api/blob/main/src/modules/cost-sharing/database/cost-sharing.prisma-repository.ts)):
  - só as de pontos `PRIVATE`; as do ponto comercial já foram pagas no cartão (ADR 0014);
  - iniciadas no mês, considerando o calendário de São Paulo;
  - `CLOSED`, ou `INTERRUPTED` com energia maior que zero, para cobrar o kWh parcial.
- **Atribuição:** a sessão herda o `unitLabel` da `Membership` do motorista no início. Dois carros da mesma unidade caem na mesma linha. Uma sessão sem unidade vai para a linha "Sem unidade".
- **Cálculo por unidade** ([`buildMonthlyStatement`](https://github.com/ev-charge-ops/api/blob/main/src/modules/cost-sharing/domain/monthly-statement.ts)):

  ```
  energia  = Σ kWh × tarifa travada de cada sessão (tarifa da concessionária)
  acesso   = taxa de acesso vigente no fim do mês, para cada unidade com motorista habilitado
  ocupação = Σ multas por ocupação das sessões
  total    = energia + acesso + ocupação
  ```

  Unidades com motorista e sem sessão no mês pagam só a taxa de acesso. As linhas são ordenadas pela unidade (ordenação natural em pt-BR) e o extrato traz os totais do condomínio.
- **Endpoints** (só para `MANAGER` da organização):

  | Endpoint | Uso |
  |---|---|
  | `GET /organizations/:id/statements?month=AAAA-MM` | extrato do mês por unidade |
  | `GET /organizations/:id/statements/export.csv?month=AAAA-MM` | o mesmo extrato em CSV |
  | `GET /organizations/:id/overview` | visão geral do mês: consumo, valores, demanda e anomalias |
  | `GET /organizations/:id/sessions` | sessões com filtros de mês, unidade, ponto, estado e anomalia |

- **CSV** ([`statement-csv.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/cost-sharing/domain/statement-csv.ts)): separador `;`, vírgula decimal, BOM UTF-8 e cabeçalho `unidade;kwh;energia;acesso;ocupacao;total`. Assim o arquivo abre direto no Excel em português.
- **Capacidade elétrica na visão geral:** demanda contratada, reserva das áreas comuns, demanda atual, média dos picos diários e alerta de aumento de demanda quando o pico médio passa de 80% do contratado ([`demand-profile.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/cost-sharing/domain/demand-profile.ts)).
- **Valores inteiros:** dinheiro em centavos e energia em Wh no domínio, para não acumular erro de arredondamento na soma das sessões.

## Consequências

- **Positivas:**
  - O fluxo sessão → consumo → rateio fica fechado e auditável: cada linha do extrato vem de sessões que o gestor consegue abrir no portal.
  - O repasse da energia é feito a custo, como exige a regulação da rede privada.
- **Negativas e riscos:**
  - O extrato é calculado na hora da consulta, e não existe um "fechamento" congelado do mês. Uma sessão encerrada depois, mas iniciada no mês, muda o extrato.
  - A taxa de acesso é cobrada por unidade com motorista, não por motorista.
  - Não há geração de boleto nem integração com a administradora. O gestor exporta o CSV.
- **Desvio da Sprint 01:** a exportação em PDF e o boleto pós-pago ficaram para depois; só o CSV foi implementado. O cartão pré-pago na rede privada também não entrou: lá a cobrança é só pelo rateio mensal.
