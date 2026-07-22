# azure-intune-labs
Para documentar laboratórios de testes em Cloud/Intune

# ☁️ Azure & Microsoft Intune - Documentação de Laboratórios e Projetos

Repositório dedicado à documentação de projetos práticos de gerenciamento de dispositivos, políticas de conformidade e identidade utilizando **Microsoft Intune** e **Microsoft Entra ID**.

---

### 📌 Projetos & Implementações

1. [Gerenciamento e Provisionamento de Dispositivos Móveis (Android Enterprise)](#1-gerenciamento-e-provisionamento-de-dispositivos-móveis-android-enterprise)
2. [Gestão de Identidade e Licenciamento (Entra ID)](#2-gestão-de-identidade-e-licenciamento-entra-id)
3. [Políticas de Conformidade e Ações de Mitigação](#3-políticas-de-conformidade-e-ações-de-mitigação)
4. [Análise de Cenários e Implantação Windows (Hybrid vs Entra ID Join)](#4-análise-de-cenários-e-implantação-windows-hybrid-vs-entra-id-join)
5. [Gestão de MDM Legado & Estratégia de Migração (Urmobo ➔ Microsoft Intune)](#5-gestão-de-mdm-legado--estratégia-de-migração-urmobo--microsoft-intune)
6. [Gestão de Telefonia Corporativa & Inventário Móvel (Telecom / TEM)](#6-gestão-de-telefonia-corporativa--inventário-móvel-telecom--tem)

---

### 1. Gerenciamento e Provisionamento de Dispositivos Móveis (Android Enterprise)

#### 🔹 Inscrição e Provisionamento (Enrollment)
- **Tablets Corporativos (Kiosk / Dedicated Devices):**
  - Implementado o perfil **Corporate-Owned Dedicated Devices**.
  - O provisionamento é iniciado via leitura do **QR Code**, exigindo a conexão manual prévia a uma rede Wi-Fi/Internet; a partir daí, o dispositivo baixa e aplica automaticamente todos os aplicativos homologados, perfis de configuração e políticas de conformidade.
  - Atribuição e aplicação de políticas feitas via **Grupos Dinâmicos** no Entra ID conforme a regra do perfil/setor (ex.: *Indústria 4.0, Kiosk Motorista, Farmácia e Segurança do Trabalho*).
- **Celulares Corporativos (COBO - Corporate-Owned, Fully Managed):**
  - Perfil **Corporate-Owned, Fully Managed User Devices**.
  - Requer pré-cadastro do usuário no grupo de segurança correspondente no Entra ID para autenticação corporativa via e-mail antes da leitura do QR Code.

#### 🔹 Perfis de Configuração (Device Configuration Profiles)
Segmentação de acesso e restrições baseadas em perfis operacionais:

- **Nível 1 (NV1 - Liberado):**
  - Recebe as aplicações obrigatórias da empresa no provisionamento.
  - Acesso à Google Play Store liberado, permitindo o download de aplicativos adicionais e fora do catálogo padrão corporativo.

- **Nível 2 (NV2 - Restrito):**
  - Recebe o pacote padrão de aplicações corporativas no provisionamento.
  - Acesso à loja restrito apenas ao catálogo de aplicações homologadas pela empresa (ex.: apps bancários, VR/VT, Microsoft 365), disponibilizadas como opcionais para instalação conforme a necessidade.

- **Restrições de Segurança Globais & Conectividade (COBO):**
  - **Conectividade:** Conexão a novas redes Wi-Fi (incluindo redes particulares/externas) liberada para garantir mobilidade.
  - **Segurança Corporativa:**
    - Bloqueio de *Factory Reset* (Reset de fábrica).
    - Bloqueio de transferência de arquivos via USB e mídias externas.
    - Bloqueio de alteração manual de Data e Hora.
    - Obrigatoriedade de Tela de Bloqueio com Padrão Mínimo de Complexidade de Senha exigido pela política de segurança.

#### 🔹 Dispositivos Dedicados (Kiosk Mode)
- **Modo:** Multi-App Kiosk.
- **Arquitetura de Autenticação:** Token do tipo *Corporate-Owned Dedicated Device* integrado ao **Microsoft Entra Shared Mode** (permite múltiplos usuários sem necessidade de senha de saída do modo Kiosk).
- **Aplicações:** Apenas apps homologados por área (ex.: rotas para motoristas, rotinas operacionais para a indústria).

#### 🔹 Gestão e Distribuição de Aplicações (Apps)
- **Obrigatórios (Required - Instalação Automática):** Microsoft Teams, Word, Waze, Authenticator, Google Chrome, entre outros.
- **Disponíveis (Available - Portal da Empresa):** Aplicações financeiras, Microsoft Loop, sistemas de chamados (ex.: Auvo).

---

### 2. Gestão de Identidade e Licenciamento (Entra ID)

- **Estruturação de Grupos:**
  - **Tablets:** Atribuição e agrupamento via **Licença por Dispositivo** (*Intune Device License*).
  - **Celulares:** Atribuição baseada em **Licenciamento por Usuário** (Mapeamento manual/atribuído devido aos requisitos de identidade corporativa).
- **Requisitos de Licenciamento Ativos:** Licenças Microsoft Intune vinculadas via planos **F1, F3, E3 ou EMS E3**.

---

### 3. Políticas de Conformidade e Ações de Mitigação

- **Requisitos de Conformidade (Compliance Policies):**
  - Exigência de senha forte/complexa e ciclo de expiração.
  - Criptografia de disco/dispositivo ativa.
- **Tratamento de Dispositivos Não-Conformes (Non-Compliant):**
  - Notificação automatizada via e-mail para o usuário final alertando sobre a inconformidade do dispositivo.
  - *Status do Projeto:* Fase de adequação do parque legado para atingir 100% de adesão às diretivas de conformidade.

---

### 4. Análise de Cenários e Implantação Windows (Hybrid vs Entra ID Join)

#### 🔹 Avaliação Técnica do Projeto
Durante a validação de arquitetura para a gestão de endpoints Windows, foram testados 3 cenários de carregamento:
1. **Entra ID Join Puro:** Incompatível com o modelo atual devido a particularidades da rede interna e ausência de VPN *Always-On* ou pré-logon para validação de domínio externo.
2. **Windows Autopilot:** Descartado momentaneamente pelo mesmo requisito de dependência da rede local para finalização do *Domain Join*.
3. **Modelo Adotado (Hybrid Entra ID Join):** Manutenção do domínio *On-Premises* gerenciado via **GPO**, combinando políticas estratégicas de nuvem via Intune.

#### 🔹 Políticas de Windows Aplicadas no Intune
- Implementação de BitLocker e gestão de chaves.
- Implantação de Certificados Digitais e perfis de Wi-Fi corporativo.
- Gestão de Administrador Local, Telemetria e Inventário de Hardware.
- *Nota de Arquitetura:* As políticas de Antivírus e Patches/Updates permanecem gerenciadas via soluções dedicadas (Trend Micro / Qualys).

---

### 5. Gestão de MDM Legado & Estratégia de Migração (Urmobo ➔ Microsoft Intune)

- **Escopo e Implantação do Parque Legado:**
  - Participação direta na implantação, configuração inicial e sustentação contínua do **Urmobo MDM** para gestão exclusiva do parque de dispositivos **Android** (celulares e tablets corporativos).
  - Gestão do ciclo de vida: criação e ajuste de perfis de restrição, publicação/inclusão de aplicativos e ajustes finos de políticas de segurança.

- **Cenário de Transição & Convivência (Parque Misto):**
  - Administração de um parque móvel de **+2.000 dispositivos**, gerenciando o cenário de convivência híbrida: **~1.400 dispositivos** mantidos no ambiente legado (Urmobo) e **~740 dispositivos** já ativos/migrados no Microsoft Intune.
  - **Estratégia de Migration Phase-Out:** Implementação da política de *freeze* no MDM antigo (bloqueio de novos *enrollments* no Urmobo) e obrigatoriedade de provisionamento via **Microsoft Intune** para todos os novos dispositivos ou aparelhos formatados/reutilizados, garantindo uma transição gradual e sem impacto operacional.

- **Racional de Negócio & Resolução de Dores Técnicas:**
  - **Otimização de Custos (FinOps):** Eliminação do custo de duplo licenciamento ao centralizar o gerenciamento no Intune (cujo custo já estava embutido no pacote de licenças Microsoft/Entra ID da empresa).
  - **Confiabilidade e Governança de Dispositivos:** Mitigação de gargalos de sincronização do MDM antigo (dispositivos que perdiam comunicação continuadamente devido a limitações de políticas padrão do SO Android, gerando riscos de segurança e falha em bloqueios remotos).
  - **Integração Nativa com Entra ID:** Alinhamento do gerenciamento móvel às políticas globais de Identidade e Acesso Condicional do ecossistema Microsoft.

---

### 6. Gestão de Telefonia Corporativa & Inventário Móvel (Telecom / TEM)

- **Gestão Operacional de Telecom (Voz e Dados):**
  - Administração do ciclo de vida de linhas móveis corporativas, incluindo ativação de **SIM cards físicos e e-SIM**, bloqueio/desbloqueio de linhas, solicitações de roaming internacional e abertura/acompanhamento de chamados junto às operadoras para resolução de falhas de sinal/conectividade.
  - Processo contínuo de **Onboarding e Offboarding** de linhas e aparelhos vinculados a colaboradores.

- **Inventário Cruzado e Governança de Ativos:**
  - Gestão e vínculo de ativos (Dispositivo x Linha x Colaborador) utilizando plataforma externa dedicada de inventário de ativos corporativos.
  - Consolidação e higienização periódica de dados cruzando informações das plataformas **Urmobo, Microsoft Intune e Inventário Externo** via planilhas e upload em massa.

- **Controle de Franquias de Dados & Políticas por Grupo:**
  - Definição e gerenciamento de cotas/franquias mensais de dados (GB/mês) por perfil de uso e setor, diretamente via portal de gestão da operadora.
  - Acompanhamento e auditoria de consumos discrepantes/excessivos de pacotes de dados.
