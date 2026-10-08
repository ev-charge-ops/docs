# ADR 0018 — Consentimento LGPD, exportação e pedido de exclusão de dados

- **Status:** aceita
- **Data:** 2026-10-08

## Contexto

O produto trata dados pessoais de moradores e visitantes: nome, e-mail, unidade, horários e energia de cada recarga e, no ponto comercial, a cobrança no cartão. O rateio expõe esses dados ao síndico, e o histórico anonimizado alimenta os modelos de IA. A Sprint 01 previa um termo de consentimento LGPD no onboarding, mas a primeira versão da Sprint 02 só tinha as páginas de privacidade e termos no portal.

Precisávamos separar o que é necessário para o serviço do que é opcional, guardar a prova do aceite por versão dos termos e dar ao titular os direitos de acesso e de exclusão (Lei 13.709/2018, art. 18).

## Decisão

- **Finalidades** ([`consent-purposes.ts`](https://github.com/ev-charge-ops/api/blob/main/src/modules/privacy/domain/consent-purposes.ts)):

  | Finalidade | Obrigatória | Uso |
  |---|---|---|
  | `ESSENTIAL_SERVICE` | sim | cadastro, vínculo e recargas, para liberar o carregador, cobrar e manter a conta |
  | `BILLING_SHARING` | sim | compartilhar energia, horário e valor das recargas com a gestão do condomínio para o rateio |
  | `USAGE_ANALYTICS` | não | histórico sem identificação para a previsão de demanda e a detecção de anomalias |
  | `MARKETING_COMMUNICATIONS` | não | novidades, dicas e ofertas |

  Os títulos e descrições em pt-BR vêm da API, então o app não duplica o texto.
- **Versão dos termos:** `GET /me/consents` devolve a versão vigente (hoje `2026-10-07`), a versão aceita pelo usuário e `mustAccept`, que fica verdadeiro enquanto as finalidades obrigatórias não forem aceitas na versão vigente. Quando os termos mudam, todos precisam aceitar de novo.
- **Registro do aceite:** `PUT /me/consents` recebe a versão e as escolhas. Enviar a versão vigente equivale a aceitar as obrigatórias. O histórico é só de inclusão (`ConsentRecord`) e grava apenas as finalidades que mudaram. Revogar uma obrigatória responde `400 REQUIRED_CONSENT`, e uma versão desatualizada responde `409 TERMS_VERSION_OUTDATED`.
- **Direitos do titular:**
  - `GET /me/data-export` devolve um JSON para download com perfil (sem hash de senha), vínculos, sessões com pagamento, histórico de consentimentos e pedidos de exclusão;
  - `POST /me/deletion-request` registra um pedido de exclusão com motivo opcional e responde `202` com o status `PENDING`. Enquanto houver um pedido pendente, a chamada devolve o mesmo pedido. A exclusão em si é feita pela equipe, fora da API.
- **No app** ([`features/privacy`](https://github.com/ev-charge-ops/mobile/tree/main/src/features/privacy)):
  - depois do login, se `mustAccept` for verdadeiro, a tela de consentimento aparece antes das abas. O registro do push e os lembretes (ADR 0017) só começam depois do aceite;
  - em Conta → Privacidade e dados, a mesma tela fica em modo de revisão: cada toggle opcional é salvo na hora e volta ao estado anterior se a API recusar;
  - "Exportar meus dados" compartilha o JSON, e "Excluir conta" abre a confirmação, com a nota de que alguns registros são mantidos por obrigação legal;
  - no cadastro, a caixa "Li e aceito os Termos de uso e a Política de privacidade" é obrigatória. O aceite formal, por finalidade, continua na tela de consentimento.
- **No portal:** as páginas `/termos` e `/privacidade` descrevem as finalidades, a base legal, os fornecedores (Vercel, Neon, Stripe, Resend, Google, Apple e Expo), a retenção e os direitos do titular, e indicam que a exportação e a exclusão são feitas pelo app.
- **Cartão:** continua só no Stripe (ADR 0014). A API guarda os dados da cobrança, não os do cartão.

## Consequências

- **Positivas:**
  - Cada aceite tem prova por versão e por finalidade, e a troca dos termos força um novo aceite.
  - O titular exporta os próprios dados e pede a exclusão pelo app, sem depender do síndico.
  - O uso dos dados para a IA tem finalidade própria e opcional.
- **Negativas e riscos:**
  - O pedido de exclusão é só registrado. A anonimização e a exclusão precisam ser feitas pela equipe, e as sessões usadas no rateio do condomínio têm de ser mantidas pelo prazo legal.
  - A escolha `USAGE_ANALYTICS` ainda não filtra os dados de treino, porque os modelos atuais foram treinados só com dados públicos (ADR 0011 e ADR 0012). O filtro passa a ser necessário quando o retreino usar o histórico do condomínio.
  - O consentimento é coletado no app. Gestores que só usam o portal não passam pela tela de aceite.
  - As páginas legais não identificam controlador nem encarregado (DPO), porque o projeto é acadêmico. O encarregado aponta para o e-mail de privacidade.
- **Desvio da Sprint 01:** o termo de consentimento do protótipo foi portado para o app, agora com finalidades separadas, histórico de aceite e os direitos de exportação e exclusão.
