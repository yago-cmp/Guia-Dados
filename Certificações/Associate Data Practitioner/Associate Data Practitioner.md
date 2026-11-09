**Data Sources** são a origem dos dados raw para dentro de um sistema ou pipeline. As fontes dos dados podem ser muitas, como sistemas Pub/Sub, Data Lakes, ou Tabelas/DBs.

**Data Sinks**, de forma oposta, são o fim, ou saída do pipeline. É através das Sinks que o consumidor final tem acesso aos dados tratados e prontos para consumo ou uso. Poderm ser usados, no GCP, o BigQuery ou o Bigtable, por exemplo.

Dados não estruturados:
[[Google Cloud/Cloud Storage|Cloud Storage]]

Dados estruturados:
[[Data Engineering/Databases]]

[[Data Engineering/Data Lake|Data Lake]]

[[Data Engineering/Data Warehouse|Data Warehouse]]

[[Google Cloud/BigQuery|BigQuery]]

[[Google Cloud/Knowledge Catalog|Knowledge Catalog]]

[[Google Cloud/Analytics Hub|Analytics Hub]]

Migracao e Replicacao:
	**gcloud storage** - batch, <1tb, on-prem, google cloud
	**Storage Tranfer** Service - batch, >1tb, on-prem, multicloud
	**Transfer Appliance** - offline batch 7tb, 40tb, 300tb, on-prem
	**Datastream** - estruturados, batch, stream, 10k tabelas por stream, on-prem, multicloud
