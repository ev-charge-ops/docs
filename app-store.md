# Publicação na App Store

Conteúdo pronto para colar no App Store Connect na submissão do app iOS do EV ChargeOps. Os blocos de código são os textos finais. O título de cada campo traz a contagem de caracteres do texto e o limite da Apple.

| Item | Valor |
|---|---|
| App no App Store Connect | `6819532299` |
| Bundle ID | `io.softmoon.evchargeops` |
| Time | Softmoon.Io (`JWSCRB8HGL`) |
| Versão | `1.4.0` (`expo.version` do [`app.json`](https://github.com/ev-charge-ops/mobile/blob/main/app.json); o build é incrementado pelo EAS) |
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
| Marketing (Marketing URL) | `https://app.evchargeops.com.br` |

### Copyright

```text
2026 Softmoon.Io
```

## 2. Preço e disponibilidade (Pricing and Availability)

- **Preço:** gratuito (Free, R$ 0,00). O app não tem compras dentro do app nem assinaturas.
- **Disponibilidade:** somente **Brasil**.
  - O app, as tarifas e os pagamentos são em português e em reais, e os pontos do piloto ficam no Brasil.
  - Liberar todos os territórios não é neutro: a União Europeia exige a declaração de status de comerciante (DSA), que publica endereço e telefone na loja, e a China continental exige registro ICP. Por isso a recomendação é manter só o Brasil.
- **Pré-encomenda:** não.
- **Distribuição em Macs com Apple silicon e no Apple Vision Pro:** desmarcar, porque o app depende de localização e mapa no iPhone.

## 3. Textos da versão 1.4.0

### Texto promocional: 147 de 170 caracteres

```text
Encontre pontos de recarga, veja o preço por kWh calculado por IA, acompanhe a recarga ao vivo e confira o rateio do condomínio, tudo em um só app.
```

### Descrição: 1.845 de 4.000 caracteres

```text
O EV ChargeOps é o app do motorista de carro elétrico para a recarga compartilhada em condomínios e em pontos públicos. Encontre um ponto livre, saiba quanto vai pagar antes de começar e acompanhe cada kWh até o recibo.

PONTOS NO MAPA
Veja os pontos de recarga do seu condomínio e os pontos públicos próximos no mapa ou em lista por distância, com status em tempo real: livre, ocupado ou fora do ar. Se o ponto estiver ocupado, entre na fila e receba um aviso quando chegar a sua vez.

PREÇO DINÂMICO COM IA
O preço por kWh considera a demanda do momento, calculada por um modelo de inteligência artificial. O valor aparece antes de você iniciar e fica travado durante toda a recarga.

RECARGA AO VIVO
Inicie a recarga pelo app, escolha o limite de bateria e acompanhe energia, potência, tempo e valor em tempo real. Encerre quando quiser. Ao terminar, você recebe o recibo detalhado, que pode ser compartilhado como imagem.

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

PROJETO PILOTO
O EV ChargeOps é um projeto piloto. Nesta fase, as recargas são simuladas. A localização dos pontos públicos vem do Open Charge Map (CC BY-SA 4.0), com preços de demonstração.
```

### Palavras-chave: 97 de 100 caracteres

```text
carro elétrico,recarga,carregador,eletroposto,veículo elétrico,condomínio,rateio,kWh,wallbox,mapa
```

### Novidades desta versão (What's New): 178 de 4.000 caracteres

```text
Primeira versão na App Store: encontre pontos de recarga no mapa, veja o preço por kWh com IA, acompanhe a recarga ao vivo, pague com cartão e confira o rateio do seu condomínio.
```

## 4. Privacidade do app (App Privacy)

- **Coleta dados?** Sim.
- **Rastreamento (tracking):** **Não** para todos os tipos. O app não usa SDK de publicidade, não acessa o IDFA e não exibe o pedido de App Tracking Transparency.
- **URL da política:** `https://app.evchargeops.com.br/privacidade`

A tabela junta os dados que a API e o app tratam com os que os SDKs de terceiros declaram nos próprios manifestos de privacidade (`PrivacyInfo.xcprivacy`). A Apple exige que o rótulo inclua os dois. Marque cada tipo com as finalidades e o vínculo abaixo.

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
| Nome | `<NOME DO RESPONSÁVEL>` |
| Sobrenome | `<SOBRENOME DO RESPONSÁVEL>` |
| Telefone | `<+55 11 9XXXX-XXXX>` |
| E-mail | `<E-MAIL DO RESPONSÁVEL>` |

### Conta de demonstração (Sign-In Information)

- **Login necessário:** sim.
- **Usuário:** `appreview@evchargeops.com.br`
- **Senha:** fornecida no campo de senha.

Antes de enviar, confira que a conta existe em produção, já aceitou os consentimentos, é moradora do condomínio de demonstração Residencial Aclimação e está no modo de cartão real com reembolso automático.

### Notas (Notes), em inglês: 2.429 de 4.000 caracteres

```text
EV ChargeOps is the driver app for shared EV charging in condominiums and at public charge points. This is a pilot: charging is simulated. No physical charger is switched on, and energy, power and time come from a simulator that runs one simulated minute per real second, so a session completes in a few minutes.

HOW TO TEST
1. Sign in with the demo account in the Sign-In Information. Sign in with Apple and Google are also available on the login screen.
2. Allow location when asked, or skip it. Location is used only while the app is open, to show nearby charge points. Without it, the map and list still work.
3. Open the Pontos tab and pick any point. A nearby point works, but any point in the list works too, since charging is simulated.
4. Condo point, no payment: filter by "Condomínio" and open L1-01 or L1-02 in Residencial Aclimação. Tap "Iniciar recarga", choose the battery limit and confirm. The cost goes to the monthly condo cost sharing statement on the Início tab.
5. Commercial point, card payment: filter by "Comercial" and open any point, such as L2-01. Tap "Iniciar recarga" and pay in the Stripe payment sheet. The demo account uses live card mode, so please use a real card. The maximum amount is pre-authorized, only the final amount is captured when the session ends, and every charge made by this account is refunded automatically.
6. Watch the live session, tap "Encerrar agora" to stop, and see the receipt. Past sessions are on the Histórico tab.

The camera is used only by the Stripe card scanner, if you choose to scan a card. Notifications remind you when charging ends and before idle fees start.

ACCOUNT DELETION
Conta tab > Privacidade e dados > Excluir conta. Deletion happens in the app and is immediate. Deleting the demo account is irreversible, so please test deletion with a new account created with Sign in with Apple. Data export is on the same screen.

PAYMENTS
Card payments pay for EV charging, a physical service consumed outside the app at a real-world charge point. Per App Review Guideline 3.1.3(e) (Goods and Services Outside of the App, formerly 3.1.5(a)), they use Stripe instead of In-App Purchase. The app sells no digital content, subscriptions or features.

DATA SOURCES
Public charge point locations come from Open Charge Map (CC BY-SA 4.0) with demo prices.

Support: https://app.evchargeops.com.br/suporte
Privacy policy: https://app.evchargeops.com.br/privacidade
```

### Anexo

Opcional: um vídeo curto gravando o fluxo de login, recarga num ponto comercial e exclusão de conta ajuda a revisão a não travar no pagamento.

## 6. Conformidade de exportação (Export Compliance)

- `ITSAppUsesNonExemptEncryption` = `false`, já definido em `ios.infoPlist` no [`app.json`](https://github.com/ev-charge-ops/mobile/blob/main/app.json).
- O app só usa a criptografia padrão do sistema (HTTPS/TLS e Keychain pelo `expo-secure-store`), que é isenta. Com a chave no Info.plist, o App Store Connect não pergunta nada a cada build.

## 7. Plano de capturas de tela

| Tamanho | Resolução (retrato) | Aparelhos de referência |
|---|---|---|
| 6,9" | 1320 × 2868 | iPhone 16 Pro Max, 17 Pro Max |
| 6,5" | 1284 × 2778 | iPhone 14 Plus, 13 Pro Max, 12 Pro Max |

- O 6,9" é o tamanho obrigatório. O 6,5" é enviado à parte para não depender do redimensionamento automático.
- Mesmas 6 telas e legendas nos dois tamanhos, nesta ordem. A legenda vai no topo da arte, em Urbanist Bold, sobre o fundo do Pulse.
- Capture com a conta `appreview@evchargeops.com.br`, com dados do Residencial Aclimação, sem informação pessoal real e com a barra de status limpa (9:41, bateria cheia).

| # | Tela | Legenda | O que mostrar |
|---|---|---|---|
| 1 | Início | Seu condomínio e seus pontos num só lugar | aba Início com o condomínio, os pontos livres e o resumo do mês |
| 2 | Pontos | Encontre pontos de recarga perto de você | aba Pontos com o mapa, os pinos de status e a lista por distância |
| 3 | Detalhe | Preço por kWh com IA, travado na recarga | detalhe de um ponto com preço, fator de demanda e botão Iniciar recarga |
| 4 | Recarga ao vivo | Acompanhe cada kWh em tempo real | sessão em andamento na tela noturna, com energia, potência, tempo e valor |
| 5 | Recibo | Recibo detalhado ao encerrar | recibo de uma recarga num ponto comercial, com valor capturado |
| 6 | Histórico | Histórico e rateio do mês da sua unidade | aba Histórico com as recargas do mês e o total da unidade |
