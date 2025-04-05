# 02 - Requisitos Arquiteturais da Solução Híbrida

Este documento apresenta os requisitos definidos para a construção da nova arquitetura híbrida da empresa XPTO. Esses requisitos foram definidos com base nas necessidades atuais da organização, na continuidade dos serviços existentes, nos objetivos da transformação digital e nas boas práticas de arquitetura de soluções.

---

## ✅ Requisitos Funcionais

Os requisitos funcionais descrevem as **funcionalidades esperadas** da solução para atender aos processos de negócio:

- Os serviços de **controle de lançamentos** e **consolidação diária** devem funcionar de forma independente, garantindo que uma falha em um não impacte o outro.
- O serviço de **consolidação diária** deve suportar até **50 requisições por segundo** durante períodos de pico, com no máximo **5% de perda aceitável**.
- A solução deve permitir o **acesso externo dos usuários** de forma segura e confiável.
- Os dados devem ser **armazenados de forma consistente** e com **acesso rápido** por meio de banco de dados e cache.

---

## 🚫 Requisitos Não Funcionais

Os requisitos não funcionais definem as **características técnicas e operacionais** da arquitetura proposta:

- **Alta disponibilidade**: a infraestrutura deve garantir operação contínua mesmo em situações de falha ou alta demanda.
- **Escalabilidade**: os serviços devem crescer horizontal ou verticalmente, conforme a carga.
- **Segurança de comunicação e dados**: todos os acessos devem ocorrer por protocolos seguros (HTTPS, SSH, VPN).
- **Controle de acesso**: deve haver autenticação robusta e autorização por perfis, seguindo o princípio do menor privilégio.
- **Automação**: a criação e manutenção da infraestrutura devem ser automatizadas com ferramentas como Terraform e Ansible.
- **Monitoramento e observabilidade**: logs, métricas e alertas devem estar disponíveis para acompanhamento e resposta a incidentes.
- **Resiliência e recuperação**: a solução deve estar preparada para falhas com estratégias de backup e recuperação de desastres (DR).
- **Governança e padronização**: a infraestrutura deve ser rastreável, auditável e documentada.

---

## 🧾 Requisitos Técnicos da Arquitetura Híbrida

- A arquitetura deve integrar **ambientes on-premises e cloud** por meio de VPN site-to-site.
- O ambiente on-premises deve ser mantido como **infraestrutura de contingência ativa**, com capacidade de assumir operações críticas em caso de indisponibilidade do ambiente principal em nuvem.
- Serviços legados devem ser mantidos operacionais no ambiente on-premises como parte da estratégia de contingência ativa, permitindo continuidade em caso de falhas na nuvem e aproveitando o investimento em infraestrutura já realizado.
- Serviços modernizados devem ser executados em **Kubernetes gerenciado** e/ou **serverless**, conforme o perfil da carga.
- A infraestrutura cloud será provisionada via **Infraestrutura como Código (IaC)**, com uso de:
  - **Terraform** para gerenciamento de recursos
  - **Ansible** para configuração de ambientes
- O acesso entre serviços deve estar segmentado por **subnets privadas**, com **controle de tráfego interno**.
- Dados sensíveis e segredos devem ser armazenados em **cofres de segredo (Secrets Manager, Vault, Key Vault)**.
- A arquitetura deve incluir **camadas de proteção contra ameaças** (WAF, segurança de API, scanners de vulnerabilidade).

---

## 🌟 Requisitos Estratégicos

- **Flexibilidade na escolha do provedor cloud**, considerando fatores como:
  - Experiência do time de TI
  - Recursos e contratos já existentes com plataformas (e-mail, colaboração)
  - Estratégia para evitar vendor lock-in e permitir futuras migrações
- **Crescimento progressivo**, com possibilidade de migração por etapas e sem interrupção da operação.
- **Visão de longo prazo**, com base na modernização contínua dos serviços e integração com pipelines CI/CD.

---

## 📌 Considerações Finais

Esta arquitetura foi desenhada para ser:

- Técnica e estrategicamente viável
- Evolutiva, com adoção progressiva da nuvem
- Automatizada e observável
- Preparada para crescimento, falhas e necessidades futuras do negócio XPTO

A definição desses requisitos serve como base para uma arquitetura híbrida **consciente, resiliente e evolutiva**, que equilibra modernização tecnológica com continuidade operacional, segurança e aproveitamento de ativos existentes.
