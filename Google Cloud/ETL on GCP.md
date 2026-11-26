
## Dataprep
Feito pela Trifacta para a transformação de dados severless e no-code. Possui suporte a diversas fontes de dados e funções pré-prontas, podendo ser combinadas e encadeadas para gerar "recipes". Ele pode ser usado em combinação com o Dataflow. Possui UI em que é possível ver os impactos das suas transformações antes de, de fato, executá-las. 

### Cloud Data Fusion
Rodando em clusters Hadoop/Spark, o Cloud Data Fusion tem como papel principal juntar diversas fontes de dados e aplicar transformações sobre elas. Ele possui uma GUI para fácil uso e definição de pipelines com transformações pré-prontas. 

### Managed Service for Apache Spark
Feito para rodar workloads em Spark dentro do GCP. Os dados em HDFS que estão no Cloud Storage são usados pelos Spark jobs, e jogam os dados processados no BQ (BigQuery Connector), no Bigtable ou em algum outro HDFS. É possível usar Worflow Templates para definir dependências entre jobs. O Spark suporta diversas linguagens como R, SQL, Python, Scala e Java. Essa versão gerenciada é especialmente interessante pois abstrai os custos e complexidades do gerenciamento específico dos clusters. Usar Serverless Batches com o gcloud cli e para automatizar jobs agendados. Interactive Sessions são ideais para uso com o JupyterLab. A plataforma de forma geral tem suporte para o BQ, external procedures e containers. A plataforma usa o Cloud Storage como armazenamento padrão.