# ADR 0009 — Domínio próprio e envio de e-mail

- **Status:** aceita
- **Data:** 2026-10-07

## Contexto

Os fluxos de autenticação dependem de e-mails confiáveis e de links estáveis. Os domínios `*.vercel.app` não servem para Universal Links/App Links nem para reputação de envio.

## Decisão

- **Domínio:** `evchargeops.com.br`, registrado no registro.br, com DNS na DigitalOcean (gerenciado via MCP `digitalocean-networking`).
  - `app.evchargeops.com.br` → portal (Vercel);
  - `api.evchargeops.com.br` → API (Vercel, região `gru1`);
  - `ml.evchargeops.com.br` → serviço de IA (Vercel);
  - a raiz redireciona com 308 para `www.`, que serve o site do produto desde 2026-10-08 ([ADR 0024](0024-landing-page-on-the-portal-project.md)). Antes, ela redirecionava para `app.`.
- **E-mail transacional:** Resend, região `sa-east-1`, remetente `EV ChargeOps <noreply@evchargeops.com.br>`.
  - Registros: DKIM (`resend._domainkey`), SPF/return-path via CNAME (`send`, `rsend`) e DMARC `p=none` para monitorar antes de endurecer.
  - O rastreamento de cliques e aberturas fica desligado, para não reescrever links com tokens.
  - A API usa a port `MailSender`: o adapter `resend` em produção e o `console` em dev/testes.
- **Apple Private Email Relay:** `noreply@evchargeops.com.br` registrado como origem, para alcançar usuários que escondem o e-mail no Sign in with Apple.
- **Banco de preview:** cada preview da Vercel ganha um branch do Neon. Um workflow apaga o branch ao fechar o PR, porque o plano gratuito tem limite de 10 branches.

## Consequências

- Links de e-mail e deep links usam um domínio único, controlado pela equipe.
- Antes de qualquer envio em volume, o DMARC deve passar para `quarantine`.
- Enquanto `MAIL_DRIVER=console`, os tokens aparecem nos logs. Por isso esse driver só é usado fora de produção.
