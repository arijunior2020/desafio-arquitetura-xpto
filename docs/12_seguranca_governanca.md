# 12 - Segurança e Governança na Arquitetura Híbrida

Este documento apresenta a estratégia de segurança e governança adotada para a arquitetura híbrida da empresa XPTO. O objetivo é garantir **proteção de dados, controle de acessos e conformidade** com boas práticas, independentemente do ambiente (cloud ou on-premises).

---

## 🔐 Princípios de Segurança Adotados

- **Zero Trust**: nenhuma entidade é confiável por padrão, mesmo dentro da rede
- **Least Privilege**: cada serviço ou usuário deve ter apenas os acessos estritamente necessários
- **Segurança em camadas** (defense in depth)
- **Criptografia em trânsito e repouso**
- **Auditoria contínua e rastreabilidade de ações**

---

## 🧭 Estratégias por Camada

| Área                        | Estratégia                                                       |
| --------------------------- | ---------------------------------------------------------------- |
| **Acesso à infraestrutura** | MFA obrigatório, bastion host com IP restrito, controle via IAM  |
| **API Gateway / Serviços**  | Autenticação OAuth2 / JWT, rate limiting, throttling             |
| **Banco de dados**          | Acesso por role + whitelisting de IP, cofre de segredos          |
| **VPN e rede híbrida**      | IPSec com criptografia forte, segmentação de subnets             |
| **CI/CD**                   | Validação de código, escaneamento de secrets e vulnerabilidades  |
| **On-Premises**             | Controle de acesso físico, criptografia de disco, backup cifrado |

---

## 🗝️ Gestão de Identidade e Acesso (IAM)

- Perfis e permissões segregados por função (Dev, Ops, Auditoria)
- Grupos de acesso com hierarquia e política de revisão periódica
- Integração com SSO (Single Sign-On), quando disponível
- Política de expiração automática para credenciais e chaves

---

## 🔒 Proteção de Dados Sensíveis

- Uso de **Secrets Manager**, **Key Vault** ou **Vault** para armazenar:
  - Senhas
  - Tokens de acesso
  - Certificados
- Criptografia AES-256 em repouso
- TLS 1.2+ para dados em trânsito
- Logging com mascaramento de informações sensíveis

---

## 📊 Auditoria, Logs e Compliance

- Logs centralizados com auditoria de:
  - Acessos administrativos
  - Alterações de infraestrutura
  - Falhas de login e acesso não autorizado
- Retenção mínima: 30 dias (infraestrutura), 90 dias (segurança)
- Repositório de logs separado com acesso somente leitura
- Ferramentas: ELK, CloudTrail, Azure Monitor, Loki

---

## ✅ Conformidade e Políticas

- **Revisão de acessos trimestral** com aprovação gerencial
- **Teste de segurança semestral** (pentest ou scan automatizado)
- Adesão a princípios da **LGPD** (dados sensíveis, consentimento)
- Checklists de segurança revisados em todo ciclo de deploy

---

## 🧠 Integração com DevSecOps

- Escaneamento automático de código (SAST, secret scan)
- Validação de containers (Trivy, Dockle)
- Linter e enforce de configurações seguras no Terraform/Ansible
- Gate de aprovação no pipeline para merge/deploy

---

## 🧪 Simulações e Respostas a Incidentes

- Simulações de ataque (red team/blue team) anuais
- Planos de resposta com RACI definido
- Checklists de contenção e comunicação interna
- Automatização de ações de correção (ex: revogação de token, lockdown de rota)

---

## 🧭 Matriz RACI – Segurança e Governança

| Atividade                                           | Dev | Ops/Infra | Segurança | Gestor TI | Compliance |
| --------------------------------------------------- | :-: | :-------: | :-------: | :-------: | :--------: |
| Provisionar infraestrutura (Terraform)              |  R  |     A     |     C     |     I     |     -      |
| Configurar VMs e serviços (Ansible)                 |  C  |     R     |     C     |     I     |     -      |
| Definir e aplicar políticas de acesso (IAM)         |  -  |     C     |     R     |     A     |     I      |
| Armazenar segredos e tokens com segurança           |  R  |     C     |     A     |     I     |     -      |
| Configurar logs e auditoria                         |  C  |     R     |     A     |     I     |     I      |
| Analisar falhas de login / acesso não autorizado    |  I  |     C     |     R     |     I     |     C      |
| Executar escaneamentos de vulnerabilidades (CI/CD)  |  R  |     C     |     A     |     I     |     -      |
| Revisar acessos e permissões                        |  -  |     C     |     R     |     A     |     C      |
| Realizar simulação de ataque / resposta a incidente |  -  |     C     |     R     |     A     |     I      |
| Aplicar políticas LGPD e retenção de dados          |  -  |     C     |     C     |     I     |     A      |

**Legenda**:  
R = Responsável pela execução  
A = Aprovador (quem toma a decisão final)  
C = Consultado (deve ser envolvido)  
I = Informado (deve ser notificado)

---

## 📌 Conclusão

A estratégia de segurança e governança da arquitetura híbrida da XPTO garante:

- Proteção robusta para dados, aplicações e infraestrutura
- Visibilidade completa e rastreabilidade de ações
- Conformidade com melhores práticas e legislações aplicáveis
- Integração nativa com os pilares de automação, observabilidade e FinOps

A segurança é tratada como um pilar contínuo e adaptável, sendo revisitada a cada nova etapa da jornada de modernização.
