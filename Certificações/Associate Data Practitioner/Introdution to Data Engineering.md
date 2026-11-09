
Build data pipelines to enable data-driven decisions

### Data Engineering Tasks and Components

![[Pasted image 20261106091515.png]]


Data source: the starting point of your data journey.
It is raw, unprocessed data waiting to be transformed into valuable insights.

The transform stage of a data pipeline represents action taken on a data source to adjust, modify, join, or customize a data source so that it matches a specific downstream data or reporting requirement.

There are three main transformation patterns:
extract and load, 
extract, load, and transform,  
extract, transform, and load.

A data sync is the final stop in the data journey.
It's where processed and transformed data is stored for future use, analysis, and decision-making.

Bq, big table are 2 examples of gcp products to storage.


#### Data Formats
Unstructured - text, image, audio
Structured - tables
Semi-structured - json, xml


#### Storage

gcs - holds your unstructured data - acsessed by http req.

* Types of google cloud storage
	* Standard: hot data, like app data
	* Nearline: database backups
	* Coldline: log files
	* Archive: Compliance

Structured data storage:
![[Pasted image 20261106093135.png]]

![[Pasted image 20261106093534.png]]

Bq: ideal for OLAP - online analytical processing
Connecting to Bq: GCP interface, GCP cli, and REST API (7 languages)

BQ stores tables into datasets structures


![[Pasted image 20261106094407.png]]

Access control is through IAM 

#### Knowledge catalog
Centrally discover, manage, monitor, and govern distributed data with 
Knowledge catalog

unified metadata, centralize security

![[Pasted image 20261106100519.png]]

Analytics hub, make sharing data across organizations easy

example of using cli for bq storage:

bq load \
--source_format=CSV \
--autodetect \
--noreplace  \
nyctaxi.2018trips \
gs://cloud-training/OCBL013/nyc_tlc_yellow_trips_2018_subset_2.csv

### Data Replication and Migration


#### Replication and migration services onboard your data into gcp

tools:
gcloud storage command (gcloud storage cp ... gs://mybucket)-> small data size
transfer appliance -> large data size
storage transfer service -> medium data size
other option: datastream or etl tools

easy, seamless migration you can use database migration service. While, if this workload is complex, you can use ETL tools like dataflow to migrate on-premise or multicloud to gcp. An option that is important to note, is the amount of data and network bandwidth impact directly on migrating time.

##### gcloud  storage cp
The data can originate from various on-premise sources like file systems, object stores, or HDFS.
Small data size

##### Storage Transfer Service
Its support multicloud, and on-premises migration to cloud storage. It's usefull to moving offline medium to large data size. it has the capability of schedulling. 

#### Datastream
Continuously replicates your RDBMS into google cloud for analytics
Enables real-time data replication from source systems 

It gets the logs of the data base and transform to an avro or json format.
Each event messages contain generic metadata (context like source table) and payload (actual data changes). Also contain souce specific metadata like databese associated with the event, schema, table and change type (the DML operation - insert, delete, update). Regardless source, datastream uses unified types to map to destination

![[Pasted image 20261106134522.png]]


┌─────────────────┐
│ Source systems     │ (On-premises / Cloud RDBMS: Oracle, MySQL, Postgres)
└────────┬────────┘
         │  1. Real-time log capture (CDC)
         ▼
┌─────────────────┐
│   Datastream         │ (Serverless CDC Replication)
└────────┬────────┘
         │  2. Writes event files (JSON / Avro)
         ▼
┌─────────────────┐
│  Cloud Storage      │ (Staging Buffer / Normalized Data)
└────────┬────────┘
         │  3. Continuous stream ingestion
         ▼
┌─────────────────┐
│    Dataflow            │ (Stream Processing / Apache Beam: transforms & orders)
└────────┬────────┘
         │  4. Real-time mutations & writes
         ▼
┌─────────────────┐
│  Cloud Spanner     │ (Target Globally Distributed Database)
└─────────────────┘

 **Architecture Steps** 

- **1. Source systems:** Primary operational databases (such as MySQL, PostgreSQL, or Oracle) where day-to-day business transactions occur.
    
- **2. CDC replication (Datastream):** A serverless Change Data Capture service that continuously reads transaction logs with low latency and minimal impact on source systems.
    
- **3. Normalized data (Cloud Storage):** A durable staging bucket that acts as a decouple buffer, receiving raw change events stored as structured files (JSON or Avro).
    
- **4. Stream processing (Dataflow):** A fully managed Apache Beam pipeline that ingests event files from Cloud Storage, handles record ordering, maps schemas, and transforms data in flight.
    
- **5. Database (Spanner):** The target globally distributed relational database that receives continuous writes with strong ACID consistency and horizontal scalability.

Architecture Steps db to (oracle OCI) 

                      ┌─────────────────────────────────┐
                      │         Source Database         │
                      │  (On-Premises / Multi-Cloud)    │
                      └────────┬───────────────┬────────┘
                              │               │
        Initial Historical Load │               │ Real-Time Changes (CDC)
        (via Data Pump Export)  │               │ (Transaction Redo/Binlogs)
                               ▼               ▼
                      ┌─────────────────┐   ┌─────────────────┐
                      │   OCI Object    │   │  OCI GoldenGate │
                      │  Storage (DMS)  │   │   (Serverless)  │
                      └────────┬────────┘   └────────┬────────┘
                               │                     │
                     Data Pump │ Import              │ Direct Apply (Replicat)
                               ▼                     ▼
                      ┌─────────────────────────────────┐
                      │    Target Database (OCI)        │
                      │  (Autonomous DB / Exadata CS)   │
                      └─────────────────────────────────┘



 **Architecture Steps** 
 
- **1. Source Database:** The active production environment (Oracle, SQL Server, MySQL, PostgreSQL) generating transactional changes (`INSERT`, `UPDATE`, `DELETE`) recorded in transaction logs.
    
- **2. Initial Load Orchestration (OCI DMS & Object Storage):** OCI Database Migration (DMS) runs an initial snapshot using Oracle Data Pump, staging the data dump into OCI Object Storage and importing it into the target database.
    
- **3. Continuous Real-Time CDC (OCI GoldenGate):** A managed, serverless change-data-capture engine that reads the source transaction logs (_Extract_) and applies changes directly to the destination (_Replicat_) in sub-second latency, bypassing the need for intermediate data pipelines (like Spark or Kafka).
    
- **4. Target Database (Autonomous DB / Exadata CS):** The target managed database on OCI, kept fully synchronized in real time until the application performs the final cutover with zero downtime.


CDC (Change Data Capture). Datastream capture the data from postgre (cloud sql) near real time using WAL - Write-Ahead Logs (transactions files). 


Datastream case:
#### PostgreSQL to BigQuery CDC with Datastream

**Architecture & Use Case** Replicate relational data from a Cloud SQL PostgreSQL database into Google BigQuery in near real-time. This pipeline establishes an automated, serverless ELT workflow for operational analytics without impacting the production database performance.

  

**Key Technical Steps**

  

1. **Networking & Security:**
    - Datastream operates in a Google-managed tenant project outside the customer's VPC.
    
    - Connectivity is established either by allowlisting Datastream's regional public egress IPs in the Cloud SQL instance or by setting up Private Connectivity (VPC Network Peering / PSC).
    
2. **Database Engine Configuration (CDC Primitives):**
    - Enable `cloudsql.logical_decoding` to convert internal binary write operations into logical row-level events.
        
    - Define table change tracking via `REPLICA IDENTITY DEFAULT` (using primary keys to track `UPDATE` and `DELETE` operations).
        
    - Grant replication permissions to the database user (`ALTER USER ... WITH REPLICATION`).
        
    - Create a `PUBLICATION` to specify which tables will emit change events.
        
    - Create a logical replication `SLOT` using the `pgoutput` plugin. The slot acts as an LSN (Log Sequence Number) bookmark, preventing PostgreSQL from purging WAL (Write-Ahead Log) segments until Datastream acknowledges receipt.
        
3. **Datastream Resources:**
    
      
    - Create two **Connection Profiles**: a source profile pointing to Cloud SQL PostgreSQL and a destination profile pointing to BigQuery.
        
          
        
    - Configure the **Stream**: combine both profiles, specify the publication and replication slot names, and select schemas/tables.
        
          
        
4. **Data Sync Lifecycle (Backfill + CDC):**
    
      
    - **Initial Backfill:** Datastream performs an initial historical dump directly from the source tables into BigQuery without requiring manual DDL table creation.
        
          
    - **Real-time Streaming:** Concurrently, Datastream attaches to the replication slot, continuously reading the WAL to stream incremental `INSERT`, `UPDATE`, and `DELETE` events into BigQuery with sub-second to low-minute latency.





### The Extract and Load Data Pipeline Pattern
Big Query:
The service provides scheduling options for recurring or on-demand transfers, along with configuration options for data source details and destination settings.

metadata caching -> increase performance of bq

![[Pasted image 20261108180048.png]]

![[Pasted image 20261108180627.png]]


![[Pasted image 20261109134751.png]]


#### Dataform
unifies data transformation, assertion, and automation in bigquery
within bq, dataform manage sql workflows

sqlx file structure:
config, js (define reusable constants and functions), pre_op (define sql to be execute before sql body), sql body (define sql execution and dependencies), post_operations (define sql to be executed after sql body)