# ADR 0024 — Site do produto no mesmo projeto do portal

- **Status:** aceita
- **Data:** 2026-10-08
- **Altera:** ADR 0009 (a raiz do domínio deixa de redirecionar para o portal)

## Contexto

A raiz `evchargeops.com.br` redirecionava para o portal (`app.`), que abre na tela de login. Para a App Store e para quem conhece o produto por um link, faltava uma página que apresentasse o app e o portal, com a empresa responsável, o link da loja e metadados para buscadores e para o compartilhamento no WhatsApp e em outras redes.

O design da página veio pronto: HTML, CSS e JS estáticos, com uma história em capítulos que rola sobre vídeos e o celular com a gravação real do app.

## Decisão

- **Mesmo projeto do portal:** a página fica em `web/public/landing`, e o projeto `web` da Vercel atende `www.evchargeops.com.br` além de `app.`. Não há repositório nem projeto novo na Vercel.
- **Roteamento por domínio** no `vercel.json`, no formato `routes`: as regras com `has: host = www.evchargeops.com.br` vêm **antes** da etapa `filesystem`. Assim `/` vira `/landing/index.html` e `/assets/*` vira `/landing/assets/*` só no `www`. O portal, os previews e o `.well-known` dos deep links não mudam.
- **Raiz:** `evchargeops.com.br` redireciona (308) para `www`, na configuração de domínio da Vercel. O registro `www` no DNS é um CNAME para a Vercel.
- **Conteúdo:**
  - capítulos do motorista com a gravação real do app; no celular, cada capítulo toca o seu trecho do vídeo em vez de acompanhar a rolagem quadro a quadro;
  - a seção "Para o condomínio", com prints reais do portal em abas;
  - razão social, CNPJ, contato e link da App Store no rodapé.
- **SEO e compartilhamento:** títulos e descrições próprios, Open Graph e Twitter Card com imagens de 1200×630 (uma para o site e outra para o portal), `robots.txt` e `sitemap.xml` por domínio e dados estruturados (`Organization`, `WebSite` e `MobileApplication`).
- **Vídeos só em MP4 (H.264):** o Safari do iPhone travava no WebM, que vinha primeiro nas fontes.

## Alternativa descartada

A primeira versão usou um Routing Middleware da Vercel (`middleware.ts`) para escolher a página pelo domínio ([web#33](https://github.com/ev-charge-ops/web/pull/33)). Em produção, o middleware falhou com `MIDDLEWARE_INVOCATION_FAILED` em todos os domínios do projeto e derrubou o portal por cerca de 10 minutos. Ele foi removido ([web#34](https://github.com/ev-charge-ops/web/pull/34)) e substituído pelas rotas por domínio ([web#35](https://github.com/ev-charge-ops/web/pull/35)), que não executam código. Antes do merge, as rotas do portal foram conferidas no preview.

## Consequências

- **Positivas:**
  - Um deploy só para o portal e o site, sem custo novo.
  - As regras por domínio não afetam o portal; um erro nelas só atingiria o `www`.
- **Negativas:**
  - O formato `routes` do `vercel.json` substitui `headers` e `rewrites`; todas as regras do portal passaram para ele.
  - Os arquivos da landing não têm hash no nome. Cada mudança de CSS ou JS precisa trocar o `?v=` da referência para furar o cache do navegador.
