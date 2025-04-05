# 10 - Estratégia FinOps (Financial Operations)

Este documento apresenta a estratégia de FinOps adotada para a arquitetura híbrida da empresa XPTO, com foco no **controle, transparência e otimização de custos** relacionados à operação de recursos na nuvem e no ambiente local.

---

## 💰 O que é FinOps?

FinOps (Cloud Financial Operations) é a prática de unir **engenharia, finanças e operações** para gerenciar os custos de computação em nuvem de forma colaborativa, contínua e orientada por dados.

---

## 🎯 Objetivos da Estratégia FinOps

- Acompanhar e **controlar os custos em tempo real**
- Evitar desperdícios e **otimizar o uso de recursos**
- Capacitar o time técnico a tomar decisões com **visibilidade financeira**
- **Prever e planejar orçamentos** baseados em uso e metas de negócio

---

## 🧩 Estratégia Aplicada ao Cenário Híbrido

| Ação                          | Cloud                                  | On-Premises                          |
| ----------------------------- | -------------------------------------- | ------------------------------------ |
| Monitoramento de uso          | CloudWatch, Azure Monitor, GCP Billing | Ferramentas locais (Zabbix, Netdata) |
| Classificação de recursos     | Tags e labels por time, ambiente       | Agrupamento por VM, sistema, setor   |
| Otimização de instâncias      | Rightsizing, escalonamento, Spot       | Reaproveitamento de recursos         |
| Automatização de desligamento | Lambda, Functions, Schedulers          | Cron jobs com Ansible                |
| Orçamento e alertas           | Budget + Alertas nativos da cloud      | Painéis de custo customizados        |

---

## 🏷️ Boas Práticas de Governança de Custos

- Uso obrigatório de **tags padronizadas**:  
  `Projeto`, `Ambiente`, `Responsável`, `CentroDeCusto`

- Separação de ambientes por **contas/projetos/subscrições**

- Alertas de consumo por:

  - Porcentagem do orçamento (ex: 80%, 90%, 100%)
  - Anomalias de uso ou spikes inesperados

- Compartilhamento de relatórios com áreas envolvidas (TI, Financeiro)

- Capacitação contínua do time com princípios FinOps

---

## 📊 Ferramentas de Suporte

| Categoria         | Ferramentas                        |
| ----------------- | ---------------------------------- |
| Monitoramento     | CloudWatch, Azure Cost Management  |
| Rightsizing       | Compute Optimizer, GCP Recommender |
| Tagging e análise | CloudHealth, OpenCost, nOps        |
| Dashboards        | Grafana + Cost Export, Power BI    |
| Planejamento      | Excel/Sheets + APIs de Billing     |

---

## 🔄 Adoção Contínua

A estratégia FinOps será adotada em fases:

1. **Fase 1 – Controle**  
   Ativar alertas, tagging e visibilidade básica por projeto

2. **Fase 2 – Otimização**  
   Ajustar instâncias, desligar inativos, padronizar deploys

3. **Fase 3 – Cultura**  
   Integrar a visão de custo no ciclo de desenvolvimento

---

## 🤝 Integração com DevOps e IaC

- Os manifestos Terraform incluirão tags obrigatórias
- O pipeline CI/CD validará uso de recursos e ambientes
- Custos serão auditáveis via versionamento e templates de uso

---

## 📌 Conclusão

A estratégia FinOps da XPTO promove:

- Visibilidade clara dos gastos em cloud e no legado
- Tomada de decisão baseada em dados
- Redução de desperdícios e melhoria de ROI
- Cultura colaborativa entre áreas técnicas e financeiras

Esse processo será evoluído de forma contínua com base no uso real da infraestrutura e nas metas de crescimento do negócio.
