# ADR 0007 — Autenticação

- **Status:** aceita
- **Data:** 2026-10-07

## Contexto

O EV ChargeOps tem dois clientes. O app mobile (Expo) é usado pelos motoristas e o portal web pelos gestores. Os dois acessam uma única API NestJS, hospedada como função serverless na Vercel com banco Neon. Precisávamos de login e cadastro com papéis (`DRIVER` e `MANAGER`), sem sessão guardada em memória no servidor e sem depender de um provedor externo nesta fase do MVP.

## Decisão

- **Senhas:** hash com Argon2id (`@node-rs/argon2`). Login com e-mail errado ou senha errada retorna o mesmo `401` genérico, e o tempo de resposta é equalizado nos dois casos.
- **Access token:** JWT HS256 com validade de 15 minutos, carregando `sub` e `role`. Fica só em memória nos clientes.
- **Refresh token:** token opaco aleatório com validade de 7 dias. A API guarda apenas o hash SHA-256 dele, na tabela `RefreshToken`. A cada `/auth/refresh` o token é rotacionado e o anterior é revogado; reapresentar um token revogado retorna `401`. O `/auth/logout` revoga o token.
- **Armazenamento nos clientes:**
  - mobile: refresh token no `expo-secure-store` (Keychain/Keystore);
  - web: refresh token no `localStorage`.
- **Autorização:** guard JWT global, com rotas públicas marcadas explicitamente, mais um guard de papéis. O portal web só atende `MANAGER`; o motorista é orientado a usar o app.
- **Contrato:** o OpenAPI é gerado pela API em `/docs-json`, e os dois clientes usam tipos gerados a partir dele (`openapi-typescript` + `openapi-fetch`).

| Endpoint | Uso |
|---|---|
| `POST /auth/register` | cadastro (papel padrão `DRIVER`) |
| `POST /auth/login` | login |
| `POST /auth/refresh` | renova a sessão, com rotação |
| `POST /auth/logout` | revoga o refresh token |
| `GET /auth/me` | usuário autenticado |

## Consequências

- **Positivas:**
  - A API continua sem estado, o que combina com o modelo serverless.
  - Um token vazado expira rápido e o reuso de refresh token é detectado.
  - Os clientes ficam tipados de ponta a ponta.
- **Negativas e riscos:**
  - **`localStorage` no web:** fica exposto a XSS. A evolução prevista é mover o refresh token para um cookie HttpOnly, o que exige mudar a API.
  - **Várias abas abertas:** a rotação pode invalidar a sessão das outras abas do portal.
  - **Gestores:** não há cadastro de gestor pelo portal. Eles são criados por seed (`npm run db:seed`) ou, no futuro, por convite.
- **Desvio da Sprint 01:** o login com Google (OAuth) previsto na Sprint 01 ficou para depois do MVP.
