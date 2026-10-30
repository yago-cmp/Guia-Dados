
### Total Cost of Ownership
É um tipo de análise feita quando é considerada a adoção à nuvem, que mede o peso da adoção ao custo de manter os sistemas  rodando on-premises. No contexto on-premises, TCO geralmente é o preço de recursos estáticos ao longo do ciclo de vida dos equipamentos. Com relação à nuvem, os custos são calculados de forma diferente: o custo é por meio de assinaturas mensais ou de pay-per-use, ao invés do custo do hardware + manutenção. Os custos on-premises também envolvem pessoal, energia e resfriamento. O **problema** de transicionar de **CapEx para OpEx: controle de custos**.

### Cloud Financial Governance
São processo que a empresa usa para governar os seus custos e depesas advindos da nuvem. A natureza volátil dos custos de cloud impactam 3 áreas: pessoas, processos e tecnologia. Para Pessoas, uma parceria entre finanças, tecnologia e negócio é essencial. Para Processos, é necessário diariamente ou semanalmente, as organizações devem analisar seus custos e o monitorar seus usos de cloud. Então, mensalmente, elas devem avaliar se esses custos são sustentáveis. Para tecnologia, o gcp oferece diversas ferramentas para monitoramento e organização de custos.

### Boas práticas de Governança de Cloud
1. **Identificar quem gerencia os custos**
	1. Define Ownership, definir POs e compartilhar os custos com os times
	2. Definir Permissões, uso de políticas para definições
	3. Criar Budgets, uso de notificações automáticas para stakeholders
2. Informação no Invoice vs Management Tools
	1. Invoice: solicitação de pagamento pelos serviços usados
	2. Management Tools: software para mostrar o porquê dos gastos
3. GCP Management Tools
	1. Visibilidade: quais recursos, por quanto, e por quem
	2. Ritmo: quem é responsável por monitorar custos com clientes?
	3. Forecast: usar a pricing calculator para medir mudanças nos custos

### Controlling Access
Hierarquia de acesso e controle no GCP:
**Resources -> Projects -> Folders -> Organizations**

**Benefícios**:
- Acesso granular, apenas no nível desejado.
- Permissões em níveis maiores tem suas propriedades herdadas pelos menores, facilitando configurações gerais ou globais.
- Usa o princípio do menor privilégio possível.
- É fácil mapear mudanças feitas nesse sistema.

### Cloud Consumption Control
O GCP possui diversas ferramentas:
- Resource Quota policies: limite por projeto ou usuário.
- Budget Threshold rules: alertas e avisos de uso caso ultrapassem um limite.
- Cloud Billing reports: resumo de uso para monitoramento.
- Dynamic Work Scheduler: definição de recursos em determinado tempo ou horário.
- Spot VMs: mais baratas, para aplicações não críticas, usam capacidade ociosa do CE.
- Commited Use Discounts: GCP Commitment, preços mais em conta para usar um mínimo.

### Developers & Operators
**Desenvolvedores** tem como objetivo escrever código rápido, lançar novas features e melhorar a experiência do cliente.
**Operators** tem como objetivo a estabilidade das plataformas, minimizando mudanças bruscas.

**DevOps**: A Ponte entre os dois, tenta diminuir barreiras entra as equipes e garantir eficiência, velocidade e a confiança no dessenvolvimento. O **SRE**, Site Reliability Engineering, cuida justamente disso, combinando aspectos dos dois lados. 4 Métricas importantes de performance:
- **Latency**: afeta a experiência do usuário, fluidez.
- **Traffic**: quantos requests chegam ao sistema, mede demanda.
- **Saturation**: mede o quão próximo da capacidade está o sistema.
- **Errors**: mede quantas exceções ou problemas ocorreram.

**SLIs**, Service Level **Indicators**: medições específicas e quantitativas sobre como um serviço está performando como Latency, Uptime e Error Rate.
**SLOs**, Service Level **Objectives**: objetivos para manter a estabilidade e confiabilidade do sistema ou serviço.
**SLAs**, Service Level **Agreements**: contrato entre o provedor e o cliente, define SLOs e penalizações em caso de não cumprimento.