# Publicação no Google Play

Conteúdo enviado ao Play Console na publicação do app Android do EV ChargeOps. A versão iOS está em [`app-store.md`](app-store.md); os textos da loja são os mesmos, com os ajustes de plataforma descritos abaixo.

| Item | Valor |
|---|---|
| Conta de desenvolvedor | Softmoon.io, conta de organização (ID `8453140454437545704`), dona `loja@softmoon.io` |
| Pacote | `io.softmoon.evchargeops` |
| Versão | `1.4.1`, versionCode 2, App Bundle (`.aab`) do perfil `production` do EAS, SDK de destino 36 |
| Assinatura | chave de assinatura do app gerenciada pelo Google; a chave do EAS é a chave de upload |
| Situação | **aprovado e publicado** em 2026-10-22: enviado em 2026-10-16 direto para produção, sem teste fechado (contas de organização não exigem) |
| Link na loja | [https://play.google.com/store/apps/details?id=io.softmoon.evchargeops](https://play.google.com/store/apps/details?id=io.softmoon.evchargeops) |
| Países | somente Brasil |
| Preço | gratuito, sem compras no app |

## 1. Conta de desenvolvedor

Para publicar como organização, o Google pediu quatro verificações:

1. **D-U-N-S** da empresa (SOFTMOON.IO SERVICOS DE INFORMATICA LTDA), que define o nome e o endereço do perfil de pagamentos.
2. **Site da organização:** `softmoon.io` verificado no Google Search Console por registro TXT no DNS da DigitalOcean.
3. **Identidade:** documento de registro da organização. O primeiro envio foi recusado porque o nome digitado não batia com o do documento; o reenvio foi aprovado.
4. **Telefones** de contato e do perfil público, confirmados por SMS depois da identidade.

## 2. Ficha da loja (pt-BR)

- **Nome:** EV ChargeOps
- **Descrição breve (78 de 80):** `Encontre pontos de recarga, veja o preço com IA e acompanhe a recarga ao vivo.`
- **Descrição completa:** a mesma da App Store ([`app-store.md`](app-store.md#descrição-1860-de-4000-caracteres)), trocando "Use e-mail e senha, código por e-mail, Google ou Iniciar sessão com a Apple" por "Use e-mail e senha, código por e-mail ou sua conta Google", porque o login com a Apple só existe no iOS.
- **Categoria:** Automóveis e veículos
- **Contato público:** `contato@softmoon.io` e `https://www.evchargeops.com.br`
- **Imagens:** ícone 512 × 512, recurso gráfico 1024 × 500 e 7 capturas de telefone 1080 × 1920 (9:16). O Google não aceita a proporção das capturas da App Store (1206 × 2622), então as mesmas telas foram recompostas com a moldura menor. Os fundos foram gerados no OpenArt, por isso as imagens foram rotuladas como criadas com IA na declaração de recursos.

## 3. Conteúdo do app

| Declaração | Resposta |
|---|---|
| Política de privacidade | `https://app.evchargeops.com.br/privacidade` |
| Anúncios | não tem |
| ID de publicidade | não usa; o AAB não declara `com.google.android.gms.permission.AD_ID` |
| Detalhes do login | conta de revisão `appreview@evchargeops.com.br`, com instruções em inglês; a senha foi colada no Play Console e não fica no repositório |
| Público-alvo | maiores de 18 anos |
| Classificação de conteúdo (IARC) | "Todos os outros tipos de apps", todas as respostas "não": **Livre** (ClassInd), PEGI 3, ESRB Livre |
| Apps governamentais, recursos financeiros, apps de saúde | não se aplica; o pagamento com cartão é de um serviço físico e é processado pela Stripe |

### Segurança dos dados

- **Compartilhamento com terceiros:** nenhum. Stripe, Google Maps e Resend atuam como prestadores de serviço, o que o Google não conta como compartilhamento.
- **Criptografia em trânsito:** sim.
- **Criação de conta:** usuário e senha com outras autenticações (código por e-mail) e OAuth (Google).
- **Exclusão de conta:** `https://app.evchargeops.com.br/suporte#excluir-conta`. A página explica a exclusão no app e, sem o app, o pedido por e-mail a `contato@softmoon.io`.

| Tipo de dado | Obrigatório | Finalidades |
|---|---|---|
| Nome | sim | funcionalidade do app, gerenciamento da conta |
| Endereço de e-mail | sim | funcionalidade do app, gerenciamento da conta |
| IDs de usuário | sim | funcionalidade, análise, segurança e prevenção de fraudes, gerenciamento da conta |
| Informações de pagamento | não (só nos pontos comerciais) | funcionalidade, segurança e prevenção de fraudes |
| Histórico de compras | sim | funcionalidade do app |
| Local aproximado e local exato | não (o usuário pode negar) | funcionalidade do app |
| Interações no app | sim | funcionalidade do app, análise |
| Registros de falhas e diagnóstico | sim | análise (declarados pelo SDK do Google Maps) |
| Identificadores do dispositivo | sim | funcionalidade do app, mensagens do desenvolvedor (token de push) |

## 4. Versão de produção

- **Notas da versão:** `Primeira versão no Google Play: encontre pontos de recarga no mapa, veja o preço por kWh com IA, acompanhe a recarga ao vivo, pague com cartão e confira o rateio do seu condomínio.`
- **Publicação gerenciada:** desligada; o app foi ao ar logo depois da aprovação.
- **Aviso do Play Console:** a otimização de código DEX (ofuscação) está abaixo do limite recomendado. Não bloqueia a publicação; ligar o R8 (`enableProguardInReleaseBuilds` e `enableShrinkResourcesInReleaseBuilds` no `expo-build-properties`) exige um build nativo novo.
- **Atualizações OTA:** o build de produção segue o canal `production` do EAS, ligado à branch `production`, o mesmo do app da App Store.

## 5. Evidências

As capturas estão em [`evidencias/google-play/`](evidencias/google-play/): a página pública do app, o recurso gráfico e a folha com as imagens enviadas.
