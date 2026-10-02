# ADR 0016 — Design system Pulse e relayout do app e do portal

- **Status:** aceita
- **Data:** 2026-10-08

## Contexto

A primeira versão do app e do portal da Sprint 02 usava fundo escuro e vermelho como cor de destaque. Esse visual tinha três problemas:

- parecia um painel técnico, e não um produto para o morador do condomínio;
- o vermelho da marca competia com o vermelho dos alertas (multa por ocupação, falha e anomalia), então o destaque e o erro tinham a mesma cor;
- o dado principal do produto, o kWh, não tinha hierarquia própria, e as telas usavam cores literais fora dos tokens.

Antes de abrir os PRs de tela, a equipe desenhou uma direção visual completa, chamada **Pulse**, num canvas com tokens, componentes, logo, protótipos de movimento e uma prancheta por tela do app e do portal: [canvas do Pulse](https://claude.ai/artifact/BWq3Na6AKkLLsnc3KUbHgR). Esse canvas é a referência de design do relayout.

## Decisão

- **Direção:** "estúdio claro". O fundo é um cinza contínuo, os cartões são brancos e planos, e o preto fica só no CTA. O carro aparece em primeiro plano, e o kWh vira o número de destaque das telas.
- **Cores com papel fixo:**

  | Papel | Uso | Preenchimento | Texto | Tint |
  |---|---|---|---|---|
  | Tinta | CTA, item ativo, cartão do total | `#111316` | branco | — |
  | Energia | carregando, ponto livre, kWh fluindo | `#3DDC84` / `#1F9D57` | `#1F7A47` | `#E1F5E9` |
  | Atenção | tolerância, pico, fila | `#F2A93B` | `#8F5A0B` | `#FCEFD8` |
  | Crítico | multa, falha, anomalia | `#E5484D` | `#B4232A` | `#FCE5E5` |
  | Informação | regras, IA, avisos neutros | `#3B82F6` | `#1E5BB8` | `#E3EDFD` |

  O verde é reservado para energia: ele só aparece onde há kWh fluindo ou ponto livre. O vermelho deixou de ser cor de marca e passou a significar só multa, falha ou anomalia.
- **Superfícies claras:** fundo `#E9EAEC`, canvas `#F3F4F5`, cartão `#FFFFFF`, inset `#F4F5F6` e linha `#DFE1E4`.
- **Superfícies noturnas:** o mapa da aba Pontos, o detalhe sobre o mapa e a recarga ao vivo (carregando, tolerância e ocupação) usam uma paleta escura própria, com base `#0E0F11`, cartão `#17181B`, elevado `#212327`, linha `#2E3036`, texto secundário `#A4A9B1`, CTA invertido em branco e verde `#3DDC84`. Cada plataforma escolhe a paleta pela superfície, não pelo tema do sistema:
  - no portal, o escopo `[data-surface="night"]` sobrescreve os tokens ([`tokens.css`](https://github.com/ev-charge-ops/web/blob/main/src/styles/tokens.css));
  - no app, `colors` e `nightColors` têm as mesmas chaves, e os componentes aceitam `scheme="night"` ([`theme.ts`](https://github.com/ev-charge-ops/mobile/blob/main/src/constants/theme.ts)). A tab bar e a StatusBar trocam de esquema junto com a tela em foco.
- **Tipografia:** Urbanist (400 a 800) para textos e números e JetBrains Mono 500 para códigos e leituras do medidor, no lugar da Nunito. A escala vai do número de destaque (88/700) ao rótulo (14/600), com algarismos tabulares nos valores que mudam ao vivo.
- **Forma e espaço:** raios 8 (chip), 14 (tile), 20 (cartão), 28 (sheet) e pílula (CTA). Espaçamento em base 4, com calha de 20 px no app e 32 px no portal. Os cartões são planos; a sombra fica para elementos flutuantes (tab bar, sidebar e sheets).
- **Logo "anel de carga":** um anel aberto (C de carga) com um raio entrando pela abertura e um ponto verde que marca a carga atual. O mesmo anel é o medidor da recarga ao vivo. Ele gera o ícone, o ícone adaptativo, a splash e o favicon do app por script ([`generate-brand-assets.mjs`](https://github.com/ev-charge-ops/mobile/blob/main/scripts/generate-brand-assets.mjs)) e o componente `Logo` do portal.
- **Movimento:**
  - durações de 140 ms (hover e toque), 320 ms (troca de tela, interpolação de valores) e 480 ms (sheets e gavetas), com `cubic-bezier(.16,1,.3,1)`;
  - casos especiais: entrada *rise* de 600 ms no app (opacidade e 18 px, em cascata de 60 ms), "plugar o cabo" em 600 ms com `cubic-bezier(.65,0,.35,1)`, anel da recarga em 1,4 s e crossfade de 400 ms entre carregando, tolerância e ocupação;
  - haptics no app nas transições da sessão (plugar, tolerância, multa e recibo);
  - **movimento reduzido:** o portal respeita `prefers-reduced-motion` e o app respeita o "reduzir movimento" do sistema (`useReduceMotion`). Sem movimento, nada anima, os números aparecem no valor final e os vídeos dão lugar ao pôster.
- **Acessibilidade e contraste** (razões calculadas pela fórmula do WCAG 2.1):
  - texto secundário `#5E636B`: 6,05:1 sobre branco e 5,02:1 sobre o fundo `#E9EAEC`. Ele é usado tanto no texto "muted" quanto no "subtle", ou seja, em todo texto pequeno;
  - `#8B9097` tem só 3,21:1 sobre branco. Ele ficou restrito a textos de 18 px ou mais (`--text-faint`) e a estados desabilitados;
  - na paleta noturna, `#A4A9B1` tem 8,12:1 sobre `#0E0F11`;
  - os textos semânticos sobre o próprio tint ficam entre 4,69:1 (energia) e 5,49:1 (informação). O verde `#1F9D57` (3,49:1 sobre branco) só é usado em preenchimentos e ícones; texto verde usa `#1F7A47` (5,34:1);
  - estado nunca é só cor: as pílulas de status têm ponto e texto, e os alertas têm ícone e título;
  - no portal, landmarks, `aria-current`, anéis de foco, abas com setas do teclado e gavetas que devolvem o foco. No app, rótulos acessíveis nos controles (inclusive a distância e o slider de limite).
- **Mídia:** vídeos e fotos gerados no OpenArt e convertidos com ffmpeg (vídeo) e sharp (imagem):
  - vídeos curtos em loop, mudos: carro em estúdio (`car-loop`), garagem (`garage-loop`) e carro visto de cima (`car-top-loop`), sempre com pôster `.webp`;
  - no portal, cada vídeo sai em WebM e MP4, nessa ordem de fonte, com no máximo 1,5 MB por arquivo (o maior, `garage-loop.webm`, tem 1,2 MB). No app, só MP4 (até 182 KB), tocado com `expo-video`;
  - fotos `.webp`: `car-hero`, `garage`, `car-top` e quatro fotos de pontos de recarga em `media/points/`, servidas pelo portal em `app.evchargeops.com.br/media` com cache longo. A API grava a URL em `ChargePoint.photoUrl` pelo seed;
  - os vídeos são decorativos (`aria-hidden`), ficam pausados quando a aba perde o foco no app e não tocam com movimento reduzido.
- **Implementação incremental:** um PR por etapa em cada repositório, começando pelos tokens e primitivos e mantendo os nomes antigos dos tokens para as telas ainda não migradas. Só a primeira etapa do app teve mudança nativa (fontes, ícone e splash), por isso o app foi para a **versão 1.3.0** com build novo. As demais etapas saíram como atualização OTA (ADR 0015).
- **Dados reais acima da prancheta:** quando a prancheta mostrava algo que a API não garante, a tela não inventa o dado. Cada PR registra essas diferenças. Algumas viraram funcionalidades da API, como o extrato do motorista, o limite em percentual, a foto dos pontos, a revisão de anomalias (ADR 0020) e os dados ao vivo da visão geral. Outras ficaram fora, como cartão salvo, NFS-e, grupo tarifário, fatores individuais do score e gráfico de 24 h por ponto.

## Consequências

- **Positivas:**
  - Cada cor tem um significado único: verde é energia, âmbar é prazo, vermelho é cobrança extra ou problema. Isso ajuda a ler a sessão ao vivo e a fila de anomalias.
  - O mesmo conjunto de tokens, raios e curvas de movimento vale para o app e o portal, e as telas deixaram de usar cores literais.
  - O texto pequeno passa no nível AA de contraste nas superfícies claras e noturnas.
- **Negativas e riscos:**
  - Há duas paletas para manter em sincronia, e cada tela precisa declarar a superfície em que está.
  - Vídeos e fontes aumentam o peso do portal e do bundle do app. O limite de 1,5 MB por vídeo, os pôsteres e o cache longo reduzem esse custo.
  - O relayout exigiu um build nativo novo (1.3.0). Quem estiver com um APK antigo precisa atualizar o app.
  - O app não tem `expo-blur`, então o detalhe sobre o mapa usa um scrim escuro sem desfoque.
- **Desvio da Sprint 01:** a identidade visual escura com vermelho, usada até a primeira versão da Sprint 02, foi substituída pelo Pulse no app e no portal. A navegação do app também mudou: abas Início, Pontos, Histórico e Conta, com um botão redondo para a recarga ao vivo, e os avisos passaram para o sino do cabeçalho.
