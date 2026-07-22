# azure-intune-labs
Para documentar laboratórios de testes em Cloud/Intune

# ☁️ Azure & Microsoft Intune - Documentação de Laboratórios e Projetos

Repositório dedicado à documentação de projetos práticos de gerenciamento de dispositivos, políticas de conformidade e identidade utilizando **Microsoft Intune** e **Microsoft Entra ID**.

---

## 📌 Projetos & Implementações

1. [Gerenciamento e Provisionamento de Dispositivos Móveis (Android Enterprise)](#1-gerenciamento-e-provisionamento-de-dispositivos-móveis-android-enterprise)
2. [Gestão de Identidade e Licenciamento (Entra ID)](#2-gestão-de-identidade-e-licenciamento-entra-id)
3. [Políticas de Conformidade e Ações de Mitigação](#3-políticas-de-conformidade-e-ações-de-mitigação)
4. [Análise de Cenários e Implantação Windows (Hybrid vs Entra ID Join)](#4-análise-de-cenários-e-implantação-windows-hybrid-vs-entra-id-join)

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
Segmentação de acesso baseada em perfis de usuários (Nível 1 e Nível 2):
- **Nível 1 (NV1):** Acesso liberado à Managed Google Play Store.
- **Nível 2 (NV2):** Loja bloqueada, permitindo apenas a instalação de aplicações homologadas (ex.: Apps bancários, VR/VT, pacotes de produtividade).
- **Restrições de Segurança Globais (COBO):**
  - Bloqueio de *Factory Reset* (Reset de fábrica).
  - Bloqueio de transferência de arquivos via USB e mídias externas.
  - Bloqueio de alteração manual de data/hora e modificações de rede/Wi-Fi.
  - Obrigatoriedade de Tela de Bloqueio com Padrão Mínimo de Complexidade de Senha.

#### 🔹 Dispositivos Dedicados (Kiosk Mode)
- **Modo:** Multi-App Kiosk.
- **Arquitetura de Autenticação:** Token do tipo *Corporate-Owned Dedicated Device* integrado ao **Microsoft Entra Shared Mode** (permite múltiplos usuários sem necessidade de senha de saída do modo Kiosk).
- **Aplicações:** Apenas apps homologados por área (ex.: rotas e telemetria para motoristas, rotinas operacionais para a indústria).

#### 🔹 Gestão e Distribuição de Aplicações (Apps)
- **Obrigatórios (Required - Instalação Automática):** Microsoft Teams, Word, Waze, Authenticator, Google Chrome.
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
