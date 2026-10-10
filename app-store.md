# Publicação na App Store

Conteúdo enviado ao App Store Connect na submissão do app iOS do EV ChargeOps. Os blocos de código são os textos que estão na loja. O título de cada campo traz a contagem de caracteres do texto e o limite da Apple.

| Item | Valor |
|---|---|
| App no App Store Connect | `6819532299` |
| Bundle ID | `io.softmoon.evchargeops` |
| Time | Softmoon.Io (`JWSCRB8HGL`) |
| Versão | `1.4.1`, build 29, do perfil `production` (`expo.version` do [`app.json`](https://github.com/ev-charge-ops/mobile/blob/main/app.json); o build é incrementado pelo EAS) |
| Situação | enviada para revisão em 2026-10-08, aguardando revisão, com lançamento automático após a aprovação ([ADR 0025](adr/0025-app-store-release.md)) |
| Idioma principal | Português (Brasil) |

## 1. Informações do app (App Information)

### Nome: 12 de 30 caracteres

```text
EV ChargeOps
```

### Subtítulo: 25 de 30 caracteres

```text
Recarga de carro elétrico
```

### Categoria

- **Principal:** Navegação (Navigation)
- **Secundária:** Utilidades (Utilities)

### Direitos de conteúdo (Content Rights)

- **O app contém, mostra ou acessa conteúdo de terceiros?** Sim.
- **Você tem os direitos necessários?** Sim. A localização dos pontos de recarga públicos vem do [Open Charge Map](https://openchargemap.org), sob a licença [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), que permite o uso com atribuição. A atribuição aparece nas páginas de [privacidade](https://app.evchargeops.com.br/privacidade), [termos](https://app.evchargeops.com.br/termos) e [suporte](https://app.evchargeops.com.br/suporte) e deve aparecer também na tela de pontos do app. Os preços desses pontos são de demonstração. O mapa usa o Google Maps SDK (`PROVIDER_GOOGLE`), que exibe a própria atribuição.

### Classificação etária (Age Rating): 4+

Responda o questionário assim:

| Seção | Pergunta | Resposta |
|---|---|---|
| Controles no app | Controles parentais (Parental Controls) | Não |
| Controles no app | Verificação de idade (Age Assurance) | Não |
| Recursos | Acesso irrestrito à web (Unrestricted Web Access) | Não |
| Recursos | Conteúdo gerado por usuários (User-Generated Content) | Não |
| Recursos | Mensagens e chat (Messaging and Chat) | Não |
| Recursos | Publicidade (Advertising) | Não |
| Temas adultos | Linguagem imprópria ou humor grosseiro | Nenhum |
| Temas adultos | Horror ou medo | Nenhum |
| Temas adultos | Álcool, tabaco ou drogas | Nenhum |
| Médico ou bem-estar | Informações médicas ou de tratamento | Nenhum |
| Médico ou bem-estar | Temas de saúde ou bem-estar | Não |
| Sexualidade ou nudez | Temas sexuais ou sugestivos, nudez | Nenhum |
| Violência | Violência de desenho, realista, prolongada ou armas | Nenhum |
| Atividades de sorte | Jogos de azar simulados, jogos de azar, concursos, loot boxes | Nenhum / Não |

Resultado esperado: **4+**. Não marque "Made for Kids".

### URLs

| Campo | URL |
|---|---|
| Política de privacidade (Privacy Policy URL) | `https://app.evchargeops.com.br/privacidade` |
| Suporte (Support URL) | `https://app.evchargeops.com.br/suporte` |
| Marketing (Marketing URL) | `https://app.evchargeops.com.br` (enviado). Desde 2026-10-08 o site do produto está em `https://www.evchargeops.com.br`, que é o melhor valor para a próxima versão |

### Copyright

```text
2026 SOFTMOON.IO SERVICOS DE INFORMATICA LTDA
```

## 2. Preço e disponibilidade (Pricing and Availability)

- **Preço:** gratuito (Free, R$ 0,00). O app não tem compras dentro do app nem assinaturas.
- **Disponibilidade:** somente **Brasil**.
  - O app, as tarifas e os pagamentos são em português e em reais, e os pontos ficam no Brasil.
  - Liberar todos os territórios não é neutro: a União Europeia exige a declaração de status de comerciante (DSA), que publica endereço e telefone na loja, e a China continental exige registro ICP. Por isso a recomendação é manter só o Brasil.
- **Pré-encomenda:** não.
- **Distribuição em Macs com Apple silicon e no Apple Vision Pro:** desmarcar, porque o app depende de localização e mapa no iPhone.

## 3. Textos da versão 1.4.1

### Texto promocional: 147 de 170 caracteres

```text
Encontre pontos de recarga, veja o preço por kWh calculado por IA, acompanhe a recarga ao vivo e confira o rateio do condomínio, tudo em um só app.
```

### Descrição: 1.860 de 4.000 caracteres

Texto enviado com a versão 1.4.1. O último parágrafo avisa que as recargas são simuladas; as páginas de suporte, termos e privacidade já não falam em projeto piloto. Para tirar esse aviso também da loja, basta editar a descrição numa próxima versão.

```text
O EV ChargeOps é o app do motorista de carro elétrico para a recarga compartilhada em condomínios e em pontos públicos. Encontre um ponto livre, saiba quanto vai pagar antes de começar e acompanhe cada kWh até o recibo.

PONTOS NO MAPA
Veja os pontos de recarga do seu condomínio e os pontos públicos de todo o Brasil no mapa ou em lista por distância, com status em tempo real: livre, ocupado ou fora do ar. Se o ponto estiver ocupado, entre na fila e receba um aviso quando chegar a sua vez.

PREÇO DINÂMICO COM IA
O preço por kWh considera a demanda do momento, calculada por um modelo de inteligência artificial. O valor aparece antes de você iniciar e fica travado durante toda a recarga.

RECARGA AO VIVO
Inicie a recarga pelo app, escolha o limite por porcentagem, kWh ou valor e acompanhe energia, potência, tempo e valor em tempo real. Encerre quando quiser. Ao terminar, você recebe o recibo detalhado, que pode ser compartilhado como imagem.

RATEIO DO CONDOMÍNIO
Nos pontos do condomínio não há cobrança no app: o consumo da sua unidade entra no rateio mensal, com a energia repassada a custo. Acompanhe o extrato do mês e o histórico de recargas.

PAGAMENTO COM CARTÃO
Nos pontos comerciais, o pagamento é feito com cartão e processado pela Stripe. O valor máximo é pré-autorizado e só o valor final é capturado ao encerrar.

AVISOS NA HORA CERTA
Receba notificações quando a recarga terminar, antes do fim do tempo de tolerância e quando a vaga precisar ser liberada.

ENTRE DO SEU JEITO
Use e-mail e senha, código por e-mail, Google ou Iniciar sessão com a Apple.

PRIVACIDADE
Gerencie seus consentimentos da LGPD, exporte seus dados e exclua a conta direto no app.

DEMONSTRAÇÃO
Nesta versão, as recargas são simuladas e os preços dos pontos públicos são de demonstração. A localização dos pontos públicos vem do Open Charge Map (CC BY-SA 4.0).
```

### Palavras-chave: 97 de 100 caracteres

```text
carro elétrico,recarga,carregador,eletroposto,veículo elétrico,condomínio,rateio,kWh,wallbox,mapa
```

### Novidades desta versão (What's New): 178 de 4.000 caracteres

A Apple não pede este campo na primeira versão publicada. Fica para a próxima.

```text
Primeira versão na App Store: encontre pontos de recarga no mapa, veja o preço por kWh com IA, acompanhe a recarga ao vivo, pague com cartão e confira o rateio do seu condomínio.
```

## 4. Privacidade do app (App Privacy)

- **Coleta dados?** Sim.
- **Rastreamento (tracking):** **Não** para todos os tipos. O app não usa SDK de publicidade, não acessa o IDFA e não exibe o pedido de App Tracking Transparency.
- **URL da política:** `https://app.evchargeops.com.br/privacidade`

Os rótulos foram publicados em 2026-10-08 com os 14 tipos abaixo. A tabela junta os dados que a API e o app tratam com os que os SDKs de terceiros declaram nos próprios manifestos de privacidade (`PrivacyInfo.xcprivacy`). A Apple exige que o rótulo inclua os dois. Marque cada tipo com as finalidades e o vínculo abaixo.

| Categoria | Tipo de dado | Finalidades | Vinculado ao usuário | Rastreamento | Origem |
|---|---|---|---|---|---|
| Informações de contato (Contact Info) | Nome (Name) | Funcionalidade do app (App Functionality) | Sim | Não | cadastro, perfil e login com Google ou Apple |
| Informações de contato (Contact Info) | Endereço de e-mail (Email Address) | Funcionalidade do app | Sim | Não | login, códigos por e-mail, convites e recuperação de senha |
| Informações de contato (Contact Info) | Número de telefone (Phone Number) | Funcionalidade do app | Sim | Não | declarado pelo SDK do Google Sign-In; o EV ChargeOps não pede telefone |
| Localização (Location) | Localização precisa (Precise Location) | Funcionalidade do app | Não | Não | só com o app aberto, para mostrar e buscar os pontos próximos; não há histórico de localização |
| Localização (Location) | Localização aproximada (Coarse Location) | Funcionalidade do app | Sim | Não | declarada pelo SDK do Google Sign-In |
| Informações financeiras (Financial Info) | Informações de pagamento (Payment Info) | Funcionalidade do app | Sim | Não | cartão informado no PaymentSheet da Stripe nos pontos comerciais; a API guarda só os IDs e os valores da cobrança |
| Identificadores (Identifiers) | ID do usuário (User ID) | Funcionalidade do app; Análise (Analytics) | Sim | Não | ID da conta, usado nas sessões, no rateio e nos tokens; Análise declarada pelo Google Sign-In |
| Identificadores (Identifiers) | ID do dispositivo (Device ID) | Funcionalidade do app; Análise | Sim | Não | declarado pelos SDKs do Google Maps e do Google Sign-In |
| Dados de uso (Usage Data) | Interação com o produto (Product Interaction) | Funcionalidade do app; Análise | Sim | Não | sessões de recarga (ponto, horários, energia e valor), detecção de anomalias e o SDK da Stripe; a análise de uso própria depende do consentimento opcional |
| Dados de uso (Usage Data) | Outros dados de uso (Other Usage Data) | Análise | Sim | Não | declarado pelo SDK do Google Sign-In |
| Diagnóstico (Diagnostics) | Dados de falhas (Crash Data) | Análise | Não | Não | declarado pelo SDK do Google Maps |
| Diagnóstico (Diagnostics) | Dados de desempenho (Performance Data) | Análise | Não | Não | declarado pelo SDK do Google Maps |
| Informações financeiras (Financial Info) | Histórico de compras (Purchase History) | Funcionalidade do app | Sim | Não | cobranças das recargas nos pontos comerciais e os valores do rateio |
| Outros dados (Other Data) | Outros tipos de dados (Other Data Types) | Funcionalidade do app; Análise | Sim | Não | condomínio, unidade e papel no condomínio; Análise declarada pelos SDKs do Google |

**O que não é coletado:**

- Câmera: usada só pelo leitor de cartão da Stripe, no aparelho, e as imagens não saem dele.
- Token de push: enviado à API apenas para entregar notificações.
- Contatos, fotos, saúde, histórico de navegação, áudio e mensagens: não são acessados.
- O próprio app não tem SDK de crash ou de desempenho (como Sentry ou Crashlytics). O diagnóstico declarado vem só do Google Maps.

Antes de enviar, gere o relatório de privacidade do build (Xcode → Organizer → Generate Privacy Report, a partir do `.xcarchive` ou do `.ipa` do EAS) e confira se nenhum SDK novo declara outro tipo de dado.

## 5. Informações para a revisão (App Review Information)

### Contato

| Campo | Valor |
|---|---|
| Nome | Rodrigo |
| Sobrenome | Dias |
| Telefone | preenchido no App Store Connect, não publicado aqui |
| E-mail | `contato@softmoon.io` |

### Conta de demonstração (Sign-In Information)

- **Login necessário:** sim.
- **Usuário:** `appreview@evchargeops.com.br`
- **Senha:** fornecida no campo de senha.

A conta existe em produção, é moradora do Residencial Aclimação (unidade "Revisão · 01") e está no modo de cartão real com estorno automático e GPS do aparelho ([ADR 0021](adr/0021-per-user-payment-and-location-modes.md)). Ela é criada pelo seed com `SEED_REVIEWER_EMAIL` e `SEED_REVIEWER_PASSWORD`; a senha não fica no repositório.

### Notas (Notes), em inglês: 574 de 4.000 caracteres

As notas explicam só o login, passo a passo, com os nomes das telas e dos botões do app. A primeira versão das notas descrevia a recarga simulada, o pagamento real com estorno e a exclusão de conta; a equipe preferiu não guiar o revisor pelo fluxo de recarga depois do pagamento.

```text
How to sign in:
1. Open the app. The "Entrar" (Sign in) screen is shown.
2. In the "E-mail" field, enter the username from the Sign-In Information.
3. In the "Senha" (Password) field, enter the password from the Sign-In Information.
4. Tap the "Entrar" button.
5. On the first sign-in, a privacy consent screen (LGPD) is shown. Tap "Aceitar e continuar" (Accept and continue).
6. If the app asks for location or notification permissions, allow or skip as you prefer.

Support: https://app.evchargeops.com.br/suporte
Privacy policy: https://app.evchargeops.com.br/privacidade
```

### Anexo

Opcional: um vídeo curto gravando o fluxo de login, recarga num ponto comercial e exclusão de conta ajuda a revisão a não travar no pagamento.

## 6. Conformidade de exportação (Export Compliance)

- `ITSAppUsesNonExemptEncryption` = `false`, já definido em `ios.infoPlist` no [`app.json`](https://github.com/ev-charge-ops/mobile/blob/main/app.json).
- O app só usa a criptografia padrão do sistema (HTTPS/TLS e Keychain pelo `expo-secure-store`), que é isenta. Com a chave no Info.plist, o App Store Connect não pergunta nada a cada build.

## 7. Capturas de tela

A App Store Connect pede hoje o tamanho de iPhone de **6,3"** (1206 × 2622), o mesmo das capturas de um iPhone 16 Pro ou 17 Pro. As versões de 6,9" (1320 × 2868) foram recusadas por dimensão inválida.

Foram enviadas 7 imagens, todas a partir de capturas reais do app, com fundo gerado no OpenArt (estúdio claro ou noturno, com o brilho verde do Pulse), título e subtítulo em Urbanist e a captura dentro de uma moldura de aparelho. Os arquivos estão em [`evidencias/app-store/`](evidencias/app-store/).

| # | Tela | Título | Subtítulo |
|---|---|---|---|
| 1 | Início | Sua recarga começa aqui | Pontos do condomínio e da rua num só app |
| 2 | Pontos (mapa) | Pontos livres perto de você | Mapa com eletropostos em todo o Brasil |
| 3 | Detalhe do ponto | Preço claro, sem margem | Tarifa da concessionária e previsão da IA |
| 4 | Iniciar recarga | Você define o limite | Por porcentagem, kWh ou valor em reais |
| 5 | Recarga ao vivo | Acompanhe ao vivo | Energia, tempo e valor em tempo real |
| 6 | Recibo | Recibo na hora | Detalhe de energia, ocupação e tarifa |
| 7 | Histórico | Seu rateio do mês | Histórico e extrato da sua unidade |

Não foi enviado vídeo de prévia: a Apple aceita só gravações de tela do próprio app, e a equipe optou por publicar sem vídeo.
