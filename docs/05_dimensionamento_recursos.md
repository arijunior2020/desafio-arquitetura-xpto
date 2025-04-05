# 05 - Dimensionamento de Recursos

Este documento apresenta uma estimativa de dimensionamento inicial de recursos computacionais para os principais componentes da arquitetura híbrida da empresa XPTO, com base nos requisitos funcionais e não funcionais levantados.

---

## ⚙️ 1. Serviço de Controle de Lançamentos

- **Ambiente**: Kubernetes gerenciado (AKS, EKS, GKE)
- **Tipo de carga**: Contínua, previsível, sensível à disponibilidade
- **Componentização**: Stateless, horizontalmente escalável

### 📌 Dimensionamento Inicial

| Recurso    | Valor Inicial Estimado     |
| ---------- | -------------------------- |
| CPU        | 2 vCPU por pod             |
| Memória    | 2 GB por pod               |
| Réplicas   | Mínimo 2 (HPA ativo)       |
| Autoescala | Até 6 réplicas por demanda |
| Storage    | PVC para logs (opcional)   |

- Habilitar **Horizontal Pod Autoscaler (HPA)** com base em uso de CPU > 70%
- Utilizar _readiness_ e _liveness probes_ para garantir alta disponibilidade

---

## ⚡ 2. Serviço de Consolidação Diária

- **Ambiente**: Serverless (ex: AWS Lambda, Azure Functions, Cloud Functions)
- **Tipo de carga**: Sob demanda, periódica, tolerante a falhas parciais
- **Requisito**: Suportar até **50 requisições por segundo**, com perda de no máximo 5%

### 📌 Configuração Inicial

| Recurso                 | Valor Estimado                 |
| ----------------------- | ------------------------------ |
| Memória                 | 512 MB a 1024 MB               |
| Timeout                 | 30s (ajustável por função)     |
| Concorrência            | 100 requisições simultâneas    |
| Política de Retentativa | Ativa para falhas transitórias |

- Ativar monitoramento com métricas de latência e taxa de erro
- Uso de filas/streams para desacoplamento pode ser considerado (ex: SQS, Service Bus)

---

## 🗄️ 3. Banco de Dados Gerenciado

- **Tipo**: Banco relacional gerenciado (RDS, Azure SQL, Cloud SQL)
- **Alta disponibilidade**: Ativada com replicação multi-AZ
- **Backup automatizado**: Retenção mínima de 7 dias

### 📌 Configuração Inicial

| Recurso       | Valor Estimado              |
| ------------- | --------------------------- |
| CPU           | 2 vCPU                      |
| Memória       | 4 GB                        |
| Armazenamento | 100 GB SSD (autoexpansível) |
| Conexões      | Pool gerenciado (20-50)     |

---

## 🚀 4. API Gateway e Camada de Entrada

- **Ambiente**: API Gateway gerenciado
- **Funções**: Autenticação, throttling, WAF, métricas

### 📌 Configuração Inicial

| Recurso            | Valor Estimado                     |
| ------------------ | ---------------------------------- |
| TPS (transações/s) | Até 100                            |
| Rate Limiting      | 60 req/min por cliente (ajustável) |
| Burst              | Até 200 req/s                      |

- Logs habilitados para auditoria e observabilidade
- Requisições podem ser roteadas para nuvem ou on-premises

---

## 🏗️ 5. Recursos Legados via Lift and Shift (IaaS)

- **Ambiente**: Máquinas Virtuais na nuvem
- **Origem**: Servidores físicos ou VMs locais replicadas

### 📌 Configuração Inicial (por VM)

| Recurso | Valor Estimado  |
| ------- | --------------- |
| CPU     | 2 vCPU          |
| Memória | 4 GB            |
| Disco   | 80 GB SSD       |
| Backup  | Snapshot diário |

- Rede privada com integração via VPN
- Acesso via SSH com IP restrito + keypair

---

## 📈 Considerações sobre Escalabilidade

- Os recursos foram dimensionados com base em estimativas conservadoras para o cenário proposto.
- A arquitetura permite crescimento por:
  - **Horizontal scaling** no Kubernetes
  - **Incremento automático de instâncias Serverless**
  - **Ajuste elástico de banco e cache**
- Toda a infraestrutura poderá ser revisada com base em métricas reais de uso após a primeira fase de operação

---

## 📌 Conclusão

O dimensionamento apresentado fornece uma base robusta para suportar os serviços da empresa XPTO em seu novo modelo híbrido, garantindo alta disponibilidade, segurança e eficiência desde a primeira entrega.
