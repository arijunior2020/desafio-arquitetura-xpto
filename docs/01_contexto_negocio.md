# 01 - Contexto de Negócio

A empresa XPTO está passando por um momento estratégico de transformação digital. Sua infraestrutura atual é totalmente on-premises, com dois serviços essenciais de um sistema de controle financeiro operando em máquinas virtuais:

- **Serviço de controle de lançamentos**
- **Serviço de consolidação diária**

Com o crescimento da base de clientes e o aumento da demanda por flexibilidade, escalabilidade e continuidade de negócios, a XPTO identificou a necessidade de migrar para uma **arquitetura híbrida**, combinando seus ativos legados com o poder e elasticidade da computação em nuvem.

---

## 🎯 Oportunidade de Modernização

A migração para a nuvem permite:

- Redução de custos operacionais com manutenção física de infraestrutura
- Ganhos em escalabilidade e performance nos períodos de pico
- Implantação de práticas modernas de segurança, observabilidade e automação
- Adoção de estratégias FinOps para otimização contínua de recursos
- Recuperação mais rápida e eficiente em caso de falhas (DR)

---

## ⚠️ Considerações sobre o Legado

No momento da elaboração desta proposta, **não foram fornecidos detalhes técnicos sobre a stack atual da aplicação legada**. Por isso, esta arquitetura assume, como premissa de trabalho, que a aplicação pode ser modernizada — seja por containerização, replatforming ou reescrita modular — e executada em ambientes gerenciados na nuvem (como Kubernetes).

No entanto, caso essa modernização **não seja tecnicamente viável no momento**, ainda é possível adotar estratégias seguras e escaláveis, tais como:

- Realizar a migração para a nuvem utilizando o padrão **Lift and Shift**, mantendo a aplicação em execução em **máquinas virtuais provisionadas em modelo IaaS**. Antes dessa movimentação, será realizado um **assessment detalhado do ambiente on-premises** para identificar a capacidade computacional necessária (CPU, memória, disco, rede), incluindo análise de compatibilidade de sistema operacional, dependências externas e requisitos de rede — garantindo que os recursos na nuvem sejam corretamente dimensionados e preparados para suportar a aplicação de forma eficiente e segura.

- Estabelecer uma **camada de intermediação moderna**, como um **API Gateway**, para permitir que novos serviços se comuniquem com o legado de forma segura, monitorável e desacoplada.

- **Manter o ambiente on-premises ativo** como parte estratégica da solução híbrida, tanto para aproveitar o investimento já realizado, quanto para atuar como **camada de contingência**. Em cenários de falha crítica na nuvem, esse ambiente pode ser acionado como alternativa de continuidade operacional, garantindo resiliência sem dependência total da cloud.

- Planejar uma modernização progressiva, utilizando o padrão **Strangler Pattern**, substituindo partes do sistema legadas por serviços modernos ao longo do tempo.
  - **Strangler Pattern**: Essa abordagem permite que novos serviços sejam desenvolvidos e implantados na nuvem, enquanto o sistema legado continua a operar. Gradualmente, as funcionalidades do sistema legado são substituídas por novos serviços, permitindo uma transição suave e minimizando o risco de interrupções.

Essas abordagens garantem que a empresa possa evoluir para uma infraestrutura moderna de forma segura, realista e gradual — **sem descartar o que já foi construído e sem depender exclusivamente da refatoração imediata do sistema legado**.

---

## 🔄 Caminho Proposto

A arquitetura proposta garante que a empresa XPTO possa:

- Integrar progressivamente sua infraestrutura legada ao ambiente cloud
- Garantir resiliência e escalabilidade com mínimo impacto ao negócio
- Automatizar e padronizar seu provisionamento com Terraform e Ansible
- Monitorar e reagir a eventos com agilidade e eficiência

Essa transição é pensada de forma segura, controlada e baseada em boas práticas de arquitetura de soluções modernas.
