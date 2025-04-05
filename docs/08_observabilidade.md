# 08 - Estratégia de Observabilidade

Este documento apresenta a estratégia de observabilidade adotada para a arquitetura híbrida da empresa XPTO, com o objetivo de garantir **visibilidade operacional completa** sobre os sistemas modernos e legados, facilitando o monitoramento contínuo, a detecção de falhas e a melhoria contínua.

---

## 🔎 O que é Observabilidade?

Observabilidade é a capacidade de **entender o que está acontecendo dentro de um sistema complexo** com base nos dados externos que ele gera. A abordagem proposta segue os **três pilares fundamentais**:

1. **Logs** – registros detalhados de eventos e comportamentos
2. **Métricas** – indicadores numéricos sobre o desempenho e uso
3. **Traces** – rastreamento de requisições ponta a ponta

---

## 🌐 Escopo da Observabilidade

A observabilidade abrangerá tanto os serviços na nuvem quanto os serviços legados executando no ambiente on-premises, incluindo:

- Aplicações modernas (Kubernetes, serverless)
- Serviços legados em VMs (IaaS e locais)
- Banco de dados e cache
- VPN e conectividade híbrida
- Infraestrutura (CPU, memória, disco, rede)
- API Gateway e Load Balancer

---

## 🧰 Ferramentas e Tecnologias

| Pilar    | Ferramenta(s)                                           | Observações                                   |
| -------- | ------------------------------------------------------- | --------------------------------------------- |
| Logs     | ELK Stack (Elastic, Logstash, Kibana) ou Loki + Grafana | Logs estruturados, com correlação de eventos  |
| Métricas | Prometheus + Grafana                                    | Coleta em tempo real, dashboards e alertas    |
| Tracing  | OpenTelemetry + Jaeger                                  | Rastreio de requisições entre serviços e APIs |
| Alertas  | Alertmanager, Grafana Alerts                            | Notificações por Slack, e-mail, PagerDuty     |
| Cloud    | CloudWatch / Azure Monitor                              | Monitoramento nativo da infraestrutura cloud  |

---

## 🚦 Estratégias por Componente

### Aplicações (modernas e legadas)

- Logs estruturados com contexto (ex: request ID, user ID)
- Métricas customizadas expostas via `/metrics` (Prometheus)
- Exporters instalados nas VMs legadas

### API Gateway e Load Balancer

- Monitoramento de tráfego, latência, erros (4xx/5xx), requisições por segundo
- Logs de acesso detalhados com mascaramento de dados sensíveis
- Dashboards com top endpoints e heatmaps de chamadas

### Banco de Dados

- Métricas de uso de CPU, conexões, tempo de resposta e locks
- Backup monitorado com logs de sucesso/falha
- Query lenta identificada por logs ou APM

### VPN / Rede

- Verificação contínua da latência entre on-premises e cloud
- Alertas de perda de pacotes e indisponibilidade de túnel
- Monitoramento da largura de banda e throughput

---

## 🔔 Alertas e Notificações

- Regras definidas com base em SLIs (ex: tempo de resposta, erro > 5%)
- Agrupamento de alertas para evitar ruído excessivo
- Escalonamento de alertas (ex: dev → líder → NOC)
- Alertas de segurança: acessos fora do padrão, falhas de autenticação

---

## 🔁 Retenção e Armazenamento

- Logs com retenção mínima de 15 dias (aplicação) e 30 dias (infraestrutura)
- Dados críticos exportados para armazenamento cold (S3, Blob, Glacier)
- Exportação de métricas históricas para análise de capacidade (FinOps)

---

## 🧪 Testes e Validação

- Simulação de falhas com chaos engineering ou ferramentas de falha controlada
- Testes periódicos de alertas e dashboards (validação visual e funcional)
- Auditoria de logs críticos com periodicidade mensal

---

## 📌 Conclusão

A estratégia de observabilidade da XPTO garante:

- **Visibilidade de ponta a ponta** entre todos os componentes do sistema híbrido
- **Detecção proativa de falhas** com alertas bem definidos
- **Facilidade de diagnóstico e recuperação**, reduzindo MTTR
- **Base sólida para decisões de capacidade, performance e segurança**

A observabilidade será continuamente aprimorada à medida que os serviços evoluírem, com base em boas práticas DevOps e SRE.
