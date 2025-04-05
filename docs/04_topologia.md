# 04 - Topologia e Fluxo de Rede

Este documento descreve a topologia lógica da arquitetura híbrida proposta para a empresa XPTO, contemplando os principais componentes, fluxos de tráfego, segmentação de redes e segurança na comunicação.

---

## 🌐 Visão Geral da Arquitetura

A arquitetura é dividida em dois domínios principais, que operam de forma integrada e segura:

- **Cloud (Nuvem Pública)**: atua como **ambiente principal de execução**, responsável por hospedar os serviços modernizados (containerizados ou serverless), banco de dados gerenciado, API Gateway, observabilidade e os serviços legados migrados via **Lift and Shift (modelo IaaS)** quando a refatoração completa não for viável de imediato.

- **On-Premises**: representa a infraestrutura local existente da XPTO, mantida de forma ativa como parte da estratégia de **contingência operacional**. Esse ambiente permite:
  - Continuidade dos serviços críticos em caso de falhas severas na nuvem;
  - Suporte a workloads legados que ainda não puderem ser migrados;
  - Apoio a testes, staging ou failover planejado;
  - Aproveitamento do investimento já realizado em hardware e licenças.

A comunicação entre esses domínios ocorre por meio de **VPN site-to-site com criptografia IPSec**, garantindo segurança e confidencialidade do tráfego.

---

## 🧱 Componentes Principais

| Camada        | Componente                                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------- |
| Entrada       | Load Balancer (HTTPS com TLS 1.2+)                                                                |
| Segurança     | API Gateway (controle de acesso, autenticação, limites)                                           |
| Execução      | Kubernetes (controle de lançamentos), Serverless (consolidação)                                   |
| Dados         | Banco relacional gerenciado (ex: RDS), Cache Redis                                                |
| Integração    | VPN site-to-site com on-premises                                                                  |
| Legado        | Serviços legados executando localmente (contingência ativa) ou migrados via Lift and Shift (IaaS) |
| Monitoramento | Prometheus, Grafana, ELK, alertas e rastreamento estruturado                                      |

---

## 🔄 Fluxo de Tráfego

```plaintext
1. O usuário acessa a aplicação via navegador ou app → Load Balancer (HTTPS)
2. O tráfego é redirecionado ao API Gateway, que:
    - Autentica e autoriza a requisição
    - Redireciona para o serviço correspondente
3. O serviço de controle de lançamentos roda no Kubernetes (cloud)
4. O serviço de consolidação é executado sob demanda via função serverless
5. Ambos os serviços acessam banco de dados e cache em subnets privadas
6. O API Gateway pode intermediar chamadas para serviços legados (cloud ou on-premises)
7. Toda comunicação com o ambiente on-premises ocorre via VPN segura
```

---

## 🛡️ Segurança na Comunicação

- Todo tráfego entre camadas e entre ambientes é criptografado (HTTPS, IPSec).
- A API Gateway atua como camada de proteção central, aplicando:

  - Autenticação com JWT ou OAuth2
  - Controle de tráfego com Rate Limiting e Throttling
  - Logs e métricas integrados a observabilidade
  - Camada WAF para proteção contra ataques comuns (ex: OWASP Top 10)

- A administração de infraestrutura ocorre via bastion host com SSH chaveada, MFA e controle de IP

---

## 🔀 Flexibilidade e Evolução

A arquitetura foi desenhada para permitir:

- Crescimento modular de workloads na nuvem
- Modernização progressiva de serviços legados
- Redução de riscos operacionais
- Suporte a múltiplos provedores cloud (design agnóstico)
- Adoção de padrões como Strangler Pattern e Blue-Green Deploy

---

## 🛡️ Resiliência e Continuidade com On-Premises

A manutenção do ambiente on-premises como parte ativa da arquitetura híbrida garante:

- Continuidade de negócios em caso de falhas graves no ambiente cloud;
- Failover planejado e controlado para serviços críticos;
- Ambiente de backup e staging local, com sincronia com a nuvem;
- Redução de riscos em migrações de sistemas críticos;
- Aproveitamento do investimento já realizado em infraestrutura física, evitando desperdício de recursos.

Essa estratégia torna a arquitetura mais robusta, confiável e preparada para cenários imprevistos, garantindo que a transformação digital ocorra de forma sustentável.

## 🧭 Visão em Diagrama (Mermaid)

```mermaid
flowchart TD
    A[Usuário / Navegador / App] --> B[Load Balancer (HTTPS)]
    B --> C[API Gateway]
    C --> D[Kubernetes - Serviço de Controle]
    C --> E[Serverless - Consolidação Diária]
    D --> F[Banco de Dados Gerenciado]
    E --> F
    D --> G[Cache Redis]
    E --> G
    C --> H[Serviços Legados na Nuvem (IaaS)]
    C --> I[Serviços Legados On-Premises]
    I <--> J[Infraestrutura Local]
    subgraph VPN
        I <--> D
    end
```
