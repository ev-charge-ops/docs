# ADR 0025 — Publicação do app na App Store

- **Status:** aceita
- **Data:** 2026-10-08
- **Complementa:** ADR 0015 (deploy e CI)

## Contexto

Até a versão 1.4.0, o app iOS só circulava pelo TestFlight, a partir do perfil `preview` do EAS. Para publicar na App Store era preciso:

- um build de loja, no canal `production`, separado dos builds de teste;
- atender às exigências de revisão da Apple: conta de demonstração, exclusão de conta, URLs de suporte e de privacidade, textos de permissão e rótulos de privacidade;
- não depender da equipe para a revisão funcionar: o carregador é simulado e o revisor está fora do condomínio.

## Decisão

- **Workflow de produção** [`release-production.yml`](https://github.com/ev-charge-ops/mobile/blob/main/.eas/workflows/release-production.yml), disparado à mão: fingerprint, build iOS com o perfil `production` (canal `production`, distribuição `store`) e envio ao App Store Connect. O Android é opcional.
- **Só builds do perfil `production` vão para a revisão.** Os builds do `deploy-preview.yml` também chegam ao TestFlight, e o primeiro envio para revisão usou um deles (build 26) por engano. O envio foi cancelado e refeito com o build de produção da 1.4.1 (build 29).
- **Revisão:** conta própria com pagamento real e estorno automático e GPS do aparelho ([ADR 0021](0021-per-user-payment-and-location-modes.md)), exclusão de conta no app ([ADR 0023](0023-account-deletion.md)), suporte, termos e privacidade públicos no portal. As notas para o revisor explicam só o login, passo a passo.
- **Ficha na loja:**
  - grátis, só no Brasil e com lançamento automático após a aprovação;
  - classificação 4+, categorias Navegação e Utilidades;
  - 7 capturas reais do app no tamanho de 6,3" (1206×2622), com fundo e título;
  - os textos estão em [`app-store.md`](../app-store.md).
- **Textos públicos sem "projeto acadêmico" ou "recarga simulada":** as páginas de suporte, termos e privacidade e a landing não citam mais o Enterprise Challenge, a parceria nem a simulação, para não confundir a revisão.
- **Identificação da empresa:** razão social e CNPJ da Softmoon no portal, nos termos, na privacidade e na landing.

## Consequências

- **Positivas:**
  - Builds de loja e de teste separados por canal: uma atualização OTA de preview não chega a quem baixou pela loja.
  - A revisão consegue usar o app sem contato com a equipe.
- **Negativas:**
  - Cada versão nativa da loja precisa de um disparo manual do workflow e de um novo envio para revisão.
  - Correções só de JavaScript para a loja precisam de uma atualização OTA no canal `production`, que hoje não é automática.
