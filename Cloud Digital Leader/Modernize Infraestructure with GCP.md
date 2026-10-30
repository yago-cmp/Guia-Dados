### **Compute Engine (VMs)**
A técnica de virtualização possibilita que diversos ambientes e sistemas operacionais operem em um mesmo hardware. 

**Compute Engine** é o **IaaS** do Google para hospedagem de márquinas virtuais, que oferece justamente esse serviço de virtualização. Na plataforma, é possível escolher os tipos de máquina e o poder computacional e capacidade de memória. O CE oferece descontos para uso continuado, e também para o comprometimento de uso.

**Spot VMs** são máquinas virtuais gerenciadas pelo CE, de forma preemptiva. Ou seja, caso o processo ou atividade performada pela VM seja concluída e ela fique ociosa, o CE pode finalizar/reiniciar a VM.

### **Cloud Run & GKE (Containers)**
Ao invés de virtualizar todo o hardware e possuir um SO próprio, os Containers são pacotes que possuem um código executável e todas as suas dependências necessárias para o funcionamento. Isso garante que o código funcione da mesma forma em diversas plataformas, ambientes e SOs diferentes. Containers são mais leves, pois abstraem acima do nível do SO, e utiliza o kernel do sistema de onde ele se encontra. Containers são um dos pilares da arquitetura de microserviços, que possui diversos serviços individualizados, containerizados, que se comunicam entre si.

**Kubernetes** é uma plataforma open-source para gerenciamento de containers. A orquestração, deploy e escalabilidade desses serviços é, em muito, facilitada.

**Google Kubernetes Engine (GKE)** é um serviço gerenciado de kubernetes do Google. Ele gerencia instâncias do Compute Engine, chamadas de Clusters. Oferece grande controle sobre o setup.

**Cloud Run** é uma outra opção para rodar serviços em containers na nuvem. É possível fazer o deploy de código simples ou containerizado para rodar na cloud sem se preocupar com escalabilidade ou gerenciamento fino de recursos. Oferece uma experiência mais simples e direta.

### **Serverless Computing**
Serverless **!= No Servers**, os servidores ainda existem, eles só não são gerenciados pelo usuário do serviço, mas sim pelo GCP. As plataformas do Google possibilitam o conceito de **Faas - Function as a Service**. Com o apertar de um botão, o código roda, os servidores são ativados para processamento, e no momento que o trabalho acaba, os servidores são desativados novamente. Faas respondem à eventos.

**Cloud Run** é utilizado para rodar aplicações stateless e que necessitam de muita flexibilidade. Os recursos podem ser escalados e desescalados quando necessário.

**Cloud Run Functions** é utilizado para alocação de funções de propósito único, baseadas em eventos. Assim, você tem **Triggers** que ativam sua execução, como alterações num banco de dados, upload de arquivos, ou mensagens Pub/Sub.

**App Engine** é utilizado como Paas para fazer o **deploy de aplicações web** escaláveis. Ela possui load balancing, versionamento e traffic splitting como funções padrão.

### **Hybrid & Multicloud**
**Google Distributed Cloud** é a plataforma para gerenciar diversas instâncias de aplicações de Kubernetes em sistemas distribuídos ou híbridos. Você pode definir instâncias em Edge Locations, mais perto dos consumidores, por exemplo. Esses clusters são gerenciados pela plataforma, e o programador não precisa se preocupar com infraestrutura ou latência, por exemplo.

Para ambientes Muticloud, o **GKE - Google Kubernetes Engine Enterpise** deixa os clusters GCP conectarem com clusters externos, como em AWS ou Azure. Isso possibilitita o GCP de atuar em cima e gerenciar todos esses clusters.

### Data Managment Solutions (Distributed Environments)
**CloudSQL** é uma âncora de dados em setups híbridos e multicloud. Possibilita a replicação de dados on-premises e uma segurança maior, para backups de dados e bancos importantes.

**BigQuery Omni** é uma solução multicloud para analisar dados de diversas fontes como GCloud, AWS e Azure. Isso evita mudanças e migrações custosas e possibilita a análise de dados distribuídos.

**Looker** é o um BI "Agnóstico de nuvem", que nos deixa visualizar os dados independentemente de sua localização.

**AlloyDB Omni** é uma versão do AlloyDB baixável para PostgreSQL que roda em qualquer ambiente, seja on-premises, na Edge ou em outras clouds.

### **Modern Architectures and Migrations**
A estrutura de **microserviços** ajuda na **escalabilidade** e no mantimento do software, fazendo com que, em um surto repentino de uso, por exemplo, a infraestrutura seja reforçada apenas aonde ela é realmente necessária. Migrando para a nuvem, o custo de infraestrutura também cai, já que você para de pagar caro nos momentos em que a demanda está baixa. É o modelo **pay-as-you-go**.

O **VMware Engine** é a plataforma de migração de instâncias VMware para o GCloud, que possibilita acesso ao BigQuery, AI/ML e GKE. O **Bare Metal Solution** é utilizado para migrar aplicações Oracle legadas, mantendo compatibilidade e funcionamento.

**Apigee API Managment** é a plataforma do GCP para operar APIs com escalabilidade, automação e segurança. Ela usa **Autenticação, Autorização e Criptografia** para segurança, um **Editor de API e um Sandbox** para desenvolvimento, **API Throtlling** para prevenir sobrecarga e um **Tracking** de uso constante para análise e reports.

#GCP #CDL 