# ADR 0023 — Exclusão da conta pelo app

- **Status:** aceita
- **Data:** 2026-10-08
- **Substitui em parte:** ADR 0018 (o pedido de exclusão deixa de ser só registrado)

## Contexto

O ADR 0018 criou o `POST /me/deletion-request`, que só registrava um pedido `PENDING` para a equipe processar. A App Store exige que um app com criação de conta permita excluí-la de dentro dele (diretriz 5.1.1(v)). Um pedido que fica parado esperando a equipe não atende a essa regra.

Ao mesmo tempo, as sessões de um morador entram no rateio do condomínio e têm de ser mantidas pelo prazo legal, e o condomínio não pode ficar sem gestor.

## Decisão

- **`DELETE /me`** ([api#42](https://github.com/ev-charge-ops/api/pull/42)), com o corpo `{ "password"?: string, "confirm": "EXCLUIR" }`:
  - a senha é obrigatória para quem tem senha (`400 INVALID_PASSWORD` se faltar ou estiver errada). Contas só com Google, Apple ou código por e-mail apenas confirmam;
  - o único gestor de uma organização recebe `409 LAST_MANAGER`, e nada é alterado;
  - tem o mesmo rate limit das rotas de autenticação.
- **O que acontece:**
  1. um e-mail "Sua conta foi excluída" é enviado para o endereço atual, antes da anonimização;
  2. numa transação, o usuário é anonimizado (nome "Usuário excluído", e-mail `deleted+<id>@evchargeops.invalid`, sem senha), e as identidades Google e Apple, os refresh tokens, os push tokens e os tokens de uso único são apagados. O pedido de exclusão vira `COMPLETED`;
  3. sessões e vínculos com o condomínio ficam, com o nome anonimizado, para o rateio;
  4. os clientes do Stripe de teste e de produção são apagados depois da transação, sem bloquear a resposta.
- **App** ([mobile#63](https://github.com/ev-charge-ops/mobile/pull/63)): em Conta → Privacidade e dados → Excluir conta, uma sheet explica o que acontece, pede a senha quando a conta tem uma e exige digitar `EXCLUIR`. Depois da exclusão, o app sai da conta e mostra "Conta excluída". A sheet antiga de pedido de exclusão saiu do app.
- O `POST /me/deletion-request` continua na API, para não quebrar versões antigas do app.

## Consequências

- **Positivas:**
  - Atende à App Store e ao direito de exclusão da LGPD sem depender da equipe.
  - O rateio do condomínio continua fechando, porque as sessões ficam com o titular anonimizado.
- **Negativas:**
  - A exclusão é irreversível e imediata; não há prazo de arrependimento.
  - Uma falha no Stripe deixa um cliente órfão no Stripe, registrado só no log.
