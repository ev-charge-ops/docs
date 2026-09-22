# ADR 0015 — Deploy e CI por repositório

- **Status:** aceita
- **Data:** 2026-10-07
- **Complementa:** ADR 0009 (domínio e banco de preview)

## Contexto

O produto tem cinco repositórios independentes (`api`, `web`, `mobile`, `ml` e `docs`) e uma equipe pequena. Cada mudança precisa ser revisada por PR, testada automaticamente e publicada sem passos manuais, e a banca precisa encontrar tudo no ar. O orçamento é zero, então usamos os planos gratuitos.

A Sprint 01 previa Docker, Redis e Railway, e o protótipo rodava numa VPS com Metro servindo bundle de desenvolvimento.

## Decisão

- **Um repositório por aplicação**, na organização [`ev-charge-ops`](https://github.com/ev-charge-ops), com o `docs` como hub. O contrato entre eles é o OpenAPI publicado pela API em `/docs-json`, do qual `web` e `mobile` geram tipos com `npm run gen:api`.
- **GitHub Actions** em todo PR e push na `main`:

  | Repositório | Verificações |
  |---|---|
  | `api` | `oxlint`, `tsc`, migrações num Postgres 17 de serviço, testes unitários e e2e (Vitest + Supertest), build |
  | `web` | `oxlint` + ESLint (fronteiras entre módulos), testes (Vitest + Testing Library + MSW), build |
  | `mobile` | `expo lint` (fronteiras entre módulos), `tsc`, testes (Jest) |
  | `ml` | `ruff check`, `ruff format --check`, `pytest` |

- **Vercel** para `api`, `web` e `ml`, com integração Git: cada PR gera um preview e cada merge na `main` vai para produção.
  - `api`: função Node na região `gru1`. O build roda `prisma generate`, `prisma migrate deploy` e `nest build`, então as migrações sobem junto com o código.
  - `web`: SPA estática do Vite, com rewrite para `index.html` e os arquivos `.well-known` de App Links e Universal Links.
  - `ml`: FastAPI em função Python. Os modelos `.joblib` versionados em `artifacts/` vão no deploy, e notebooks e dados ficam de fora (`.vercelignore`).
- **Neon Postgres** com um branch de banco por preview da `api`, criado pela integração Neon + Vercel e apagado quando o PR fecha pelo workflow [`cleanup-preview-database.yml`](https://github.com/ev-charge-ops/api/blob/main/.github/workflows/cleanup-preview-database.yml). Assim, cada PR testa as migrações num banco isolado.
- **EAS Workflows** no `mobile` ([`deploy-preview.yml`](https://github.com/ev-charge-ops/mobile/blob/main/.eas/workflows/deploy-preview.yml)): a cada push na `main`, o fingerprint nativo é calculado por plataforma.
  - Fingerprint novo, ou seja, mudança nativa: build novo, com APK Android de distribuição interna e build iOS enviado ao TestFlight.
  - Fingerprint já existente, ou seja, mudança só de JavaScript: atualização OTA (EAS Update) no canal `preview`, sem novo build.
  - Cada PR publica uma OTA no branch do PR para revisão ([`publish-pr-update.yml`](https://github.com/ev-charge-ops/mobile/blob/main/.eas/workflows/publish-pr-update.yml)).
- **Segredos** ficam nas variáveis de ambiente da Vercel e da EAS, nunca no repositório. Os arquivos `.env.example` documentam cada variável.

## Consequências

- **Positivas:**
  - Nenhum servidor para manter. Produção e previews sobem sozinhos a cada merge, e a banca usa as mesmas URLs que a equipe.
  - Mudanças de JavaScript chegam aos aparelhos em minutos, sem passar pela loja.
  - Bancos de preview isolados evitam que um PR estrague os dados da demonstração.
- **Negativas e riscos:**
  - Funções serverless não guardam estado entre requisições: o rate limit fica em memória por instância (ADR 0008) e não há processo em segundo plano (ADR 0010).
  - O plano gratuito do Neon limita os branches a 10, daí a limpeza automática.
  - A cota de builds na nuvem do plano gratuito da EAS é limitada. Quando ela acaba, a OTA é publicada da máquina de um integrante com `eas update --branch preview` depois de cada merge.
  - O iOS depende de convite no TestFlight. O Android é instalado pelo APK.
- **Desvio da Sprint 01:** saíram Docker, Redis e Railway, e entraram Vercel, Neon e EAS. A VPS do protótipo, que servia o bundle de desenvolvimento pelo Expo Go, foi substituída por builds reais e atualizações OTA.
