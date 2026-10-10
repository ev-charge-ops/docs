# ADR 0022 — Rede pública do Open Charge Map e mapa em escala

- **Status:** aceita
- **Data:** 2026-10-08

## Contexto

O mapa do app mostrava os 3 pontos do condomínio e 12 pontos comerciais fictícios em São Paulo. Para a revisão da App Store e para quem usa o app fora da Aclimação, o mapa precisava de uma rede de recarga real no Brasil inteiro.

Duas restrições vieram junto:

- `GET /charge-points` devolvia todos os pontos comerciais e calculava o preço de cada um, com uma chamada ao `ml` por ponto. Com milhares de pontos, isso fica lento e pesado no celular.
- O app 1.4.0 já instalado chama a lista sem filtro e não pode receber milhares de itens.

## Decisão

- **Fonte:** o [Open Charge Map](https://openchargemap.org) (OCM), sob a licença CC BY-SA 4.0, que exige atribuição. A ideia inicial era chegar a cerca de 15 mil pontos raspando outro site, mas a equipe preferiu uma fonte aberta com licença clara.
- **Importação** ([api#41](https://github.com/ev-charge-ops/api/pull/41)), num script separado do seed (`npm run db:import-ocm`):
  - busca os pontos por caixa de cada UF, dividindo a caixa quando ela chega ao limite de resultados;
  - um operador do OCM vira uma organização `COMMERCIAL` com tarifa de demonstração; operadores desconhecidos vão para "Rede pública · UF";
  - um ponto por POI, com `source = OCM` e `externalId`, carregador simulado e foto de demonstração;
  - a gravação é idempotente: rodar de novo só atualiza coordenadas e status que mudaram;
  - o detalhe dos pontos OCM traz a atribuição "Dados de localização © Open Charge Map (CC BY-SA 4.0)", e o app mostra o selo "Preço de demonstração".
- **Resultado em produção:** 1.784 pontos de 64 operadores em 25 UFs (SP 358, GO 251, RJ 193, DF 184 e MG 107, entre outras). O Amapá e o Acre não têm pontos no OCM. Não completamos o número com pontos inventados.
- **API do mapa** ([api#40](https://github.com/ev-charge-ops/api/pull/40)):
  - `GET /charge-points?bbox=…&limit=` devolve um item leve, só com os pontos da área visível, ordenados do centro para fora (padrão 300, máximo 1000). O preço usa o fator de demanda em cache por operador e hora, sem chamar o modelo por ponto;
  - `GET /charge-points/clusters?bbox&zoom` agrega os pontos numa grade no próprio SQL, para os zooms de país e de estado;
  - um índice em `(latitude, longitude)` e um cache LRU do fator de demanda;
  - **compatibilidade:** sem `bbox`, a resposta continua igual para o app 1.4.0, mas limitada aos pontos do condomínio do usuário mais os comerciais a até 25 km, com no máximo 200.
- **App** ([mobile#62](https://github.com/ev-charge-ops/mobile/pull/62)): busca por área visível com espera de 300 ms, clusters do servidor no zoom 9 ou menor, agrupamento no aparelho com `supercluster` acima de 150 pontos, no máximo 200 pins desenhados e lista paginada pelos mais próximos.

## Consequências

- **Positivas:**
  - O mapa mostra recarga pública real no país inteiro, com licença e atribuição corretas.
  - No teste local com 15 mil pontos, a área de São Paulo respondeu em 15 ms e os clusters do Brasil em 26 ms (medianas).
  - O app antigo continua funcionando, com uma lista menor.
- **Negativas:**
  - A cobertura depende do OCM: 1.784 pontos, bem abaixo dos cerca de 15 mil estimados para o Brasil.
  - Os preços dos pontos OCM são de demonstração, e os carregadores desses pontos também são simulados.
  - Os filtros do app valem só para os pontos já carregados; os clusters do servidor não são filtrados.
