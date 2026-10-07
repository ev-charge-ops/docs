# ADR 0008 — Fluxos de autenticação e convites

- **Status:** aceita
- **Data:** 2026-10-07
- **Complementa:** ADR 0007 (autenticação base)

## Contexto

Login por e-mail e senha não basta para o produto. Moradores esquecem a senha, preferem entrar com Google ou Apple e chegam ao condomínio por convite do gestor. Os links enviados por e-mail precisam funcionar tanto no portal web (gestores) quanto no app (motoristas).

## Decisão

- **Tokens de uso único:** tabela `OneTimeToken` com os tipos `EMAIL_VERIFICATION`, `PASSWORD_RESET` e `EMAIL_LOGIN`.
  - Guardamos só o hash.
  - Emitir um token novo invalida o anterior do mesmo tipo.
  - Os códigos de 6 dígitos são comparados em tempo constante e travam após 5 tentativas.
- **Verificação de e-mail:** o cadastro envia um link válido por 24 h, e o OAuth já chega com o e-mail verificado.
- **Recuperação de senha:** `forgot` responde sempre `202`, sem revelar se a conta existe, com tempo mínimo de resposta. O `reset` (30 min, uso único) troca a senha e revoga todas as sessões.
- **Login sem senha:** código de 6 dígitos + link mágico, válidos por 10 min. Só funciona para contas existentes.
- **OAuth:** `POST /auth/oauth/google` e `POST /auth/oauth/apple` recebem o ID token, que é validado com `jose` contra o JWKS do provedor (`iss`, `aud` dentro da lista de client IDs e `email_verified`).
  - Se a identidade já está vinculada, faz login.
  - Se o e-mail verificado corresponde a uma conta existente, vincula a identidade a ela.
  - Senão, cria uma conta `DRIVER` sem senha.
  - A tabela `UserIdentity` registra provedor e `subject`.
- **Organizações e convites:** `Organization` (condomínio) e `Membership` com papel `MANAGER`/`DRIVER` por organização.
  - O gestor cria, reenvia e revoga convites (válidos por 7 dias).
  - O convidado aceita pela página pública `/invite`, criando senha, ou já autenticado, desde que o e-mail coincida.
  - Convites expirados, revogados ou usados retornam `410`.
- **Deep links:** os e-mails apontam para `https://app.evchargeops.com.br/<rota>?token=`. O `web` publica `apple-app-site-association` e `assetlinks.json`, e o app declara Associated Domains e App Links para `/invite`, `/reset-password`, `/login/email` e `/verify-email`. Sem o app instalado, a página web é o fallback.
- **Rate limit:** guard próprio, em memória por instância: 100 req/min no geral e 10 req/min nas rotas de autenticação, com `429` + `Retry-After`. O `@nestjs/throttler` foi descartado porque quebra no runtime ESM da Vercel.

## Consequências

- Fluxos completos de conta sem depender de provedor de identidade externo pago.
- O rate limit em memória não é compartilhado entre instâncias serverless. Para produção real, o caminho é um store compartilhado (Redis/Upstash).
- Contas criadas só via OAuth não têm senha, mas podem criar uma pelo fluxo de recuperação.
- **Desvio da Sprint 01:** o OAuth previsto entrou também com Apple (exigência da App Store quando há login social), e entrou o login sem senha, que não estava no plano.
