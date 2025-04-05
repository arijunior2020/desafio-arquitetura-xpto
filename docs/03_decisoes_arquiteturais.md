# 03 - Decisões Arquiteturais

Este documento apresenta as decisões técnicas e estratégicas adotadas para a construção da arquitetura híbrida da empresa XPTO, com base nos requisitos levantados e nas melhores práticas de engenharia de infraestrutura moderna.

---

## 🏗️ Modelo de Arquitetura Híbrida

A solução proposta adota uma **arquitetura híbrida consciente**, onde a nuvem torna-se o ambiente principal de operação, mas o ambiente on-premises é mantido como parte da estratégia de **resiliência e continuidade**, aproveitando o investimento realizado e garantindo operação em caso de falhas na nuvem.

### Justificativa

- Permite **reaproveitamento de investimentos** já realizados em infraestrutura local.
- Garante **transição progressiva** para a nuvem, sem interrupções no negócio.
- Oferece **redundância e resiliência** por meio de comunicação segura entre os ambientes.
- Viabiliza escalabilidade de componentes modernos sem depender de um refactoring completo do legado.
- Mantém o ambiente on-premises como **camada de contingência ativa**, garantindo resiliência adicional e continuidade de serviços críticos em caso de falhas severas na nuvem.

---

## ☁️ Critérios para Escolha da Nuvem

Embora a arquitetura seja neutra em relação ao provedor (AWS, Azure, GCP), a escolha prática deve considerar:

- **Experiência do time interno** com plataformas específicas
- **Recursos já contratados** com provedores (e-mail, colaboração, suporte)
- **Possibilidade de agregar serviços sob o mesmo contrato**, otimizando custos
- **Evitar vendor lock-in**, mantendo componentes portáveis e multiplataforma

A arquitetura proposta é **agnóstica** e compatível com múltiplos provedores, reduzindo dependências estratégicas e facilitando uma eventual mudança futura.

---

## ⚙️ Execução dos Serviços

### Serviço de Controle de Lançamentos

- Deploy em **cluster Kubernetes gerenciado** (AKS, EKS ou GKE)
- Workload contínuo e resiliente, com escalabilidade automática (HPA)

### Serviço de Consolidação Diária

- Deploy como **função serverless** ou Job Kubernetes
- Executado sob demanda, com tolerância a falhas leves

### Camada de Banco de Dados

- Instância gerenciada de banco relacional (RDS, Azure SQL, etc.)
- Replicação entre zonas de disponibilidade
- Cache Redis para redução de latência

---

## 🔐 Segurança e Comunicação

- Comunicação interna entre ambientes via **VPN site-to-site (IPSec)**
- Acesso externo via **HTTPS com TLS 1.2+**
- Administração com **acesso SSH restrito**, MFA e IPs autorizados
- Gestão de segredos com **Secrets Manager / Key Vault**

Mais detalhes sobre segurança e governança estão descritos no arquivo `12_seguranca_governanca.md`.

---

## 🧰 Ferramentas e Automação

### Terraform

- Utilizado para o **provisionamento e gerenciamento da infraestrutura** cloud (VPCs, subnets, instâncias, banco de dados, etc.)
- Permite versionamento, reusabilidade e rastreabilidade

### Ansible

- Utilizado para o **gerenciamento de configuração** das instâncias e VMs (ex: pacotes, arquivos, permissões, variáveis de ambiente)
- Complementa o Terraform com foco em pós-provisionamento

---

## 🧠 Justificativa da Adoção de Kubernetes e Serverless

| Critério      | Kubernetes                    | Serverless                              |
| ------------- | ----------------------------- | --------------------------------------- |
| Tipo de Carga | Contínua, previsível          | Eventual, sob demanda                   |
| Controle      | Alto controle e customização  | Baixa complexidade operacional          |
| Custos        | Custos estáveis e previsíveis | Custos otimizados por execução          |
| Casos de Uso  | Serviço principal             | Consolidação, integrações, agendamentos |

---

## 🔄 Estratégia para o Legado

Caso a aplicação atual **não possa ser modernizada de imediato**, será adotada uma estratégia de transição segura e controlada baseada em **Lift and Shift no modelo IaaS**, precedida por um **assessment técnico detalhado** do ambiente on-premises. Esse assessment irá identificar:

- A capacidade computacional ideal (CPU, memória, disco, rede)
- Dependências do sistema (banco de dados, serviços externos, SO)
- Riscos e restrições que impactem na portabilidade da aplicação

Com base nessa análise, será feita a **migração do sistema legado para máquinas virtuais na nuvem**, replicando o ambiente atual com melhorias de segurança, automação e escalabilidade básica.

Para **viabilizar a integração da aplicação legada com novos serviços modernos**, será implantada uma **camada de intermediação por API Gateway**, que terá papel fundamental na:

- Autenticação e autorização das chamadas (ex: tokens JWT, OAuth2)
- Limitação e controle de tráfego (**rate limiting**, **throttling**)
- Transformação de payloads, padronizando os formatos entre cliente e legado
- Registro de logs, métricas e observabilidade de requisições
- Aplicação de regras de segurança como firewall de aplicação (WAF)

Essa camada permitirá que **novos sistemas e aplicações consumam funcionalidades do legado de forma segura, desacoplada e evolutiva**, ao mesmo tempo em que o backend original segue operando no modelo tradicional.

Por fim, será adotada uma abordagem de **integração progressiva**, com monitoramento contínuo, coleta de métricas e análise de viabilidade para refatoração futura — usando padrões como o **Strangler Pattern** para modernização por partes, sem impacto direto no negócio.

**Importante:** mesmo após a migração de parte dos serviços legados para a nuvem via Lift and Shift, o ambiente on-premises continuará sendo mantido como parte da **estratégia de contingência ativa**. Isso garante não apenas o aproveitamento do investimento já realizado, mas também oferece **resiliência operacional**, permitindo que a XPTO mantenha continuidade de negócio mesmo diante de falhas críticas no ambiente principal.

Mais detalhes sobre esse cenário estão descritos no arquivo `01_contexto_negocio.md`.

---

## 📌 Conclusão

As decisões arquiteturais apresentadas aqui garantem:

- Sustentação da operação atual
- Evolução segura para a nuvem
- Governança, segurança e escalabilidade
- Suporte a uma estratégia contínua de transformação digital

Essa solução foi pensada para ser executável no curto prazo, evolutiva no médio e flexível no longo prazo.
