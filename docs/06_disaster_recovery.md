# 06 - Estratégia de Disaster Recovery (DR)

Este documento apresenta a estratégia de Disaster Recovery (recuperação de desastres) para a arquitetura híbrida da empresa XPTO, com foco em garantir a continuidade operacional mesmo diante de falhas críticas na nuvem ou em componentes da infraestrutura local.

---

## 🌪️ O que é Disaster Recovery?

Disaster Recovery (DR) é o conjunto de práticas, processos e ferramentas utilizadas para garantir que sistemas de TI possam ser restaurados ou continuem operando em caso de eventos disruptivos, como:

- Queda de datacenter (cloud ou on-premises)
- Falhas de conectividade prolongadas
- Ataques cibernéticos
- Erros de configuração graves

---

## 🧭 Estratégia Baseada na Arquitetura Híbrida

A XPTO adota uma arquitetura híbrida, onde:

- A **cloud pública** atua como **ambiente principal** de produção
- O ambiente **on-premises é mantido como contingência ativa**, pronto para ser ativado em cenários críticos

Essa abordagem permite manter operações essenciais mesmo se a nuvem ficar indisponível, aproveitando a infraestrutura local já existente como fallback planejado.

---

## 📐 Definições de RTO e RPO

| Métrica | Significado                                                                    | Valor Estimado |
| ------- | ------------------------------------------------------------------------------ | -------------- |
| **RTO** | Recovery Time Objective - tempo máximo aceitável para restabelecer os serviços | Até 1 hora     |
| **RPO** | Recovery Point Objective - perda máxima aceitável de dados                     | Até 15 minutos |

Esses valores podem ser refinados conforme maturidade operacional e automações adicionais forem evoluídas.

---

## 🧰 Ferramentas e Técnicas Utilizadas

- **Backups automatizados**:

  - Banco de dados com snapshots diários + PITR (point-in-time recovery)
  - Storage em nuvem com versionamento ativado

- **Sincronização entre ambientes**:

  - Sincronização de dados críticos do banco com instância on-premises (replicação assíncrona)
  - Upload de artefatos e configurações para repositório seguro

- **Infraestrutura como Código (IaC)**:

  - Recriação automatizada do ambiente na nuvem ou em outro provedor, se necessário
  - Versionamento completo via Terraform e Ansible

- **Monitoramento e alertas proativos**:
  - Logs e métricas com notificações automáticas de falhas
  - Testes de failover periódicos (simulações de indisponibilidade)

---

## 🔁 Possíveis Estratégias de Failover

| Cenário                            | Ação Proposta                                                    |
| ---------------------------------- | ---------------------------------------------------------------- |
| Nuvem indisponível                 | Ativação manual/semi-automática dos serviços legados on-premises |
| Falha apenas em parte dos serviços | Redirecionamento via API Gateway para caminhos alternativos      |
| Banco de dados inacessível         | Restauração via snapshot recente ou uso de réplica local         |

---

## 🧭 Fluxo de Recuperação de Desastres (Mermaid)

```mermaid
flowchart TD
    A[Ambiente Cloud (Primário)] -->|Falha detectada| B{Verificar impacto}
    B -->|Parcial| C[Redirecionar via API Gateway]
    B -->|Total| D[Ativar ambiente On-Premises]
    D --> E[Serviços legados operando localmente]
    E --> F[Sincronização com último backup]
    F --> G[Serviços disponíveis até restauração cloud]

    subgraph Automação
        H[Terraform] --> I[Recriação Infraestrutura]
        J[Ansible] --> K[Reconfiguração de serviços]
    end

    G --> H
    G --> J
```

---

## 🧪 Testes de DR e Melhoria Contínua

- Simulações de indisponibilidade devem ser executadas trimestralmente
- Avaliação de tempo real de recuperação em cada ciclo
- Aprendizados de cada teste devem alimentar o plano de resposta a incidentes

---

## 📌 Conclusão

A estratégia de DR proposta permite à XPTO manter continuidade operacional mesmo em cenários adversos, com:

- Recuperação rápida de serviços essenciais
- Redução da perda de dados
- Planejamento baseado na arquitetura híbrida
- Aproveitamento do ambiente on-premises como parte do plano de resiliência

O plano pode ser aprimorado continuamente com testes regulares, automação de processos e maior integração com ferramentas de segurança e backup.
