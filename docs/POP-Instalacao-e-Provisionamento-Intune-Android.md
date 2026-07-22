# 📋 POP - Instalacao e Provisionamento de Dispositivo Android via Microsoft Intune

**Objetivo:** Orientar a equipe de TI no processo completo de inclusão de novos dispositivos móveis Android no Microsoft Intune, desde a atribuição de permissões/grupos no Entra ID até a finalização do *setup* no aparelho.  
**Escopo:** Dispositivos móveis (*Smartphones/Tablets*) corporativos (cenários COBO / Kiosk).

---

### 📱 Parte 1: Pré-requisitos e Atribuição de Grupo no Entra ID

Antes de iniciar a configuração física do dispositivo, o aparelho/usuário deve estar corretamente categorizado no **Microsoft Entra ID (Azure AD)** para receber as políticas e aplicativos adequados ao seu setor.

1. **Acesso ao Portal do Entra ID:**
   - Acesse o portal administrativo ([entra.microsoft.com](https://entra.microsoft.com/)).
   - Navegue até **Groups** ➔ **All groups**.

2. **Atribuição do Grupo Correto:**
   - Pesquise pelo grupo correspondente ao perfil de uso ou setor do dispositivo:
     - `GRP-INTUNE-ANDROID-OPERACIONAL` *(Apenas apps essenciais / Modo Kiosk)*
     - `GRP-INTUNE-ANDROID-ADMINISTRATIVO` *(Apps de produtividade + M365)*
     - `GRP-INTUNE-ANDROID-LOGISTICA` *(Apps específicos de bipagem/rastreio)*
   - Entre no grupo selecionado, acesse **Members** ➔ **Add members** e adicione a conta de e-mail/usuário associada ao dispositivo.

---

### 📲 Parte 2: Passo a Passo no Dispositivo Android

Após a garantia de que o usuário/dispositivo está no grupo correto, inicie o processo no aparelho físico.

#### Step 1: Leitura do QR Code de Token
1. Ligue o dispositivo na tela de **Boas-Vindas (Welcome)**.
2. Toque repetidamente (6 vezes) em uma área vazia da tela inicial para ativar o leitor de QR Code do Android Enterprise.
3. Aponta a câmera para o QR Code de *enrollment* fornecido pelo portal do Intune.

#### Step 2: Conexão de Rede
1. Na tela de redes disponíveis, selecione uma rede **Wi-Fi com acesso à internet**.
2. Insira a credencial da rede e aguarde a verificação de atualizações do sistema.

#### Step 3: Aceite dos Termos de Serviço
1. O dispositivo exibirá a tela *"Este dispositivo é gerenciado por sua organização"*.
2. Clique em **Avançar** e aceite os termos de uso do Google e do Microsoft Intune.

#### Step 4: Autenticação de E-mail Corporativo
1. Na tela de login da organização, digite a conta de **e-mail corporativo** do usuário responsável pelo aparelho.
2. Insira a senha temporária ou realize a validação de segundo fator (MFA), se solicitado.

#### Step 5: Definição de Segurança e Bloqueio de Tela
1. O sistema exigirá a conformidade com a política de senhas da empresa.
2. Defina o padrão de segurança exigido (**PIN numérico de 6 dígitos** ou **Senha Alfanumérica**).
3. Biometria (impressão digital/reconhecimento facial) pode ser configurada nesta etapa, caso permitido.

#### Step 6: Carga Inicial de Aplicativos e Tela Principal
1. O Android iniciará o download e instalação automática do **Microsoft Intune Agent / Company Portal** e dos aplicativos vinculados ao grupo do Entra ID.
2. Aguarde até ser redirecionado para a **Tela Inicial (Home)** do Android.
3. Na tela inicial, os aplicativos homologados começarão a ser baixados em segundo plano. O processo está finalizado quando o ícone do **Portal da Empresa** apresentar o status de sincronizado.
