Essa seção trata de Dados, e como eles agregam e potencializam as decisões das empresas.

## **Tipos de Dados**
***
- **Estruturados**
     Dados organizados e bem definidos, normalmente salvos e postos em tabelas, com relacionamentos. Exemplos são bancos de dados ou planilhas. Essa estruturação deixa eles relativamente fáceis de serem analisados.
- **Não Estruturados**
	  Dados que não seguem um modelo predefinido, que não são organizados de nenhuma maneira. Texto, Arquivos como imagens, áudios e vídeos e atividade de infraestrutura e dados de performance caem sobre essa categoria. ML é usado para categorizar e reconhecer padrões nesse tipo de dados, assim como algumas APIs.
- **Semi Estruturados**
	  Dados organizados em uma certa hierarquia, mas sem uma ordem ou diferenciação completa. Alguns exemplos são E-mails, HTML, JSON, XML, etc. Embora não exista uma estrutura formal, muitas vezes eles possuem tags para análises melhores.

## **Origem dos dados**
- **First-party Data**
	  Agregados de dados que uma empresa coleta dentro de suas próprias plataformas, como dos próprios usuários, transações ou interações.
- **Second-party Data**
	  São dados de parceiros da empresa, ou seja, First-party data de alguma empresa parceira ou organização. São dados não autorais, mas que são relevantes para a empresa em questão.
- **Third-party Data**
	  São dados que são gerenciados por um terceiro externo à organização, como dados públicos ou que acabam indo para o mercado (data brokers).
## **Data Warehouse**
Ao contrário de uma database comum, que é utilizada para armazenamento, consulta e uso, uma Data Warehouse é estruturada para permitir análises e relatórios melhores. Ela concatena diversas fontes de dados diferentes em um grande hub para análise dos dados. Uma DW é um dos componentes essenciais para o BI. Ela lida primariamente com dados estruturados e semi-estruturados, e não é a primeira opção quando estamos lidando com dados não estruturados. **BigQuery**.

## **Data Lake**
Um Data Lake guarda dados de formatos variados, sem necessariamente uma estrutura ou formato em comum, de forma "crua", ou seja, da forma como chegaram. Essa estratégia tenta prevenir casos de envenenamento dos dados, ao mesmo tempo em que tenta enriquecer dados já existentes apresentando outros relacionados, construindo contexto. **Cloud Storage**.

## **Unstructured Data Storage**
***
Google **Cloud Storage** é um serviço de armazenamento em nuvem de objetos. A mudança de "arquivos" para "objetos" se deve ao fato de que arquivos nos remetem à hierarquia, como pastas e diretórios. Os Objetos são guardados em binário, junto com metadados relevantes e um id global (uma URL). O Cloud Storage é um serviço altamente escalável e completamente gerenciado, servindo a diversos usos.
#### **Storage Classes**
- **Standard Storage**
	 Dados quentes, acessados frequentemente, além de dados temporários (usados por pouco tempo e removidos).
- **Nearline Storage**
	  Dados não tão frequentes, mas que ainda necessitam de acesso ~mensal (30 dias). Pode incluir backups ou conteúdo multimídia grande.
- **Coldline Storage**
	  Opção de baixo custo para dados frios, ou seja, de uso infrequente. Acesso e alterações em média em 90 dias.
- **Archive Storage**
	  Opção de baixíssimo custo. Dados acessados menos de uma vez por ano, e possui taxas de acesso mais altas.
#### **Autoclass**
O Autoclass é uma feature que automaticamente direciona os objetos para uma classe de armazenamento julgada adequada, com base nas médias de frequência de acesso.

### **Structured Data Storage**
***
Para dados estruturados, temos Schemas que os mapeiam para os containers específicos.

O **Cloud SQL** é um cliente em nuvem para DBMSs como MySQL, PGSQL, SQLServer, etc que tem como objetivo transferir algumas consultas ou tarefas repetitivas em SQL para a Cloud.

O **AlloyDB** é um serviço para PostgreSQL que melhora o desempenho do DBMS, usando camadas mais inteligentes de armazenamento.

**Spanner** é um serviço gerenciado de SQL para sistemas críticos, que escala horizonalmente. Ele oferece segurança e um uptime quase contínuo.

O **BigQuery** possui dois serviços: de armazenamento e de análise. É um Data Warehouse que lida automaticamente com infraestrutura, escalabilidade e segurança. Oferece consultas em SQL. Possui compatibilidade com ML e é ideal para volumes massivos de dados.

### **Semi-Structured Data Storage**
***
O **Firestore** é um banco de dados NoSQL de tempo real, muito utilizado para melhorar a responsividade de sistemas e apps mobile e websites. Guarda Collections, que possuem Documents (os dados em si). Suporta uma grande gama de formatos. Ele faz um cache offline dos dados para depois sincronizar com o DB em suas implementações.

O **Bigtable** é o serviço NoSQL que possui baixíssima latência e alto volume de dados, ideal para aplicações que fazem o uso intensivo de dados. É bem usado em IoT.

### **Data Streaming/Ingestion**
***
 Pub/Sub é um serviço de mensagens distribuído que pode receber mensagens de diversas fontes. Ele é ideal para grandes fluxos de dados que devem ser recebidos em tempo real. Quando o subscriber recebe os dados, é necessária uma forma de direcionar eles para uma Data Warehouse para futura análise. É aí que entra o Dataflow, que reune tanto os dados advindos de streaming quando de batch. Ele cria pipelines para processar esses inputs.


#GCP  #CDL 