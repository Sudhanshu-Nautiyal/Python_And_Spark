# Overview of AWS Databases

## OLTP vs OLAP

| Parameters | OLTP | OLAP |
| --------- | ---- | ---- |
| **Meaning** | Online transaction processing | Online Analytics processing |
| **Functionality** | Online database modifying system | Online database query management system |
| **Purpose** | Real time business operations | Analysis of business measures by category and attributes |
| **Characteristic** | Large numbers of short online transactions | Large volume of data |
| **Style** | Fast response time, low data redundancy and is normalized | Created uniquely to integrate different data sources for building a consolidated database |
| **Design** | Application oriented e.g. Banking, ticket booking, SMS delivery etc. | Subject oriented e.g. for sales / marketing / purchasing etc. |
| **Query type** | Standardized and simple | Complex queries involving aggregations |
| **Operation** | Read/write | Only read and rarely write |
| **Tables** | Normalized | Not normalized |
| **Method** | DBMS | Data warehouse |
| **AWS Services** | Aurora, RDS | Redshift |

## Choose right database

| Category | Question | Deeper |
| -------- | -------- | ------- |
| Workload | Read-heavy, write-heavy, or balanced workload? | • Throughput needs? High or low throughput? • Will it change, does it need to scale or fluctuate during the day? |
| Size | How much data to store and for how long? | • Will it grow? • Average object size? • How are they accessed? Security needs? |
| Durability | Data durability (e.g. for a week or forever)? | • Source of truth for the data? |
| Latency | Latency requirements? | • Concurrent users?
| Model | Data model? | • How will you query the data? Primary key? Joins? • Structured? Semi-structured? |
| Schema | Strong schema or more flexibility? | Reporting? Search? RDBMS / NoSQL? |
| Licensing | License costs? | Switch to Cloud Native DB such as Aurora? |

## Database Types

- All Database types comes with backup / restore feature.

| Type | Databases | Use cases |
| ---- | --------- | --------- |
| RDBMS | RDS, Aurora | SQL, OLTP, Joins, Data in tabular form
| Key-value | DynamoDB (also Document ≈JSON), ElastiCache | No joins & more performance and scalability |
| Object Store| S3, Glacier | Big objects (S3), backups / archives (Glacier)
| Data Warehouse | Redshift, Athena | SQL Analytics, BI, OLAP (Redshift)
| Search | Elastic Search (JSON) | Free text, unstructured searches |
| Graphs | Neptune | Relationships between data. |

## Comparison

| Database | Type | Use-case | Operations | Security | Reliability | Performance | Cost |
| -------- | ---- | -------- | ---------- | -------- | ----------- | ----------- | ---- |
| **RDS** | RDBMS | Relational datasets (RDBMS, OLTP), transactional SQL queries. | • Small downtime if failover, maintenance, scaling replicas / EC2, restore EBS • Changes requires application changes | • AWS -> OS / EC2 • Users -> KMS, SG, IAM policies, user auth with e.g. IAM, SSL | Multi AZ feature with auto-failover | • Up to 5 read replicas • Depends on EC2, EBS, read replicas • no auto-scaling | Pay per hour based on provisioned EC2 and EBS |
| **Aurora** | RDBMS | Same as RDS with less maintenance and more performance & flexibility | Less operations than RDS e.g. auto scaling storage | Same as RDS | • Multi AZ • More HA than RDS • Serverless option for more reliability | • 5x than RSD • Up to 15 read replicas | • Pay per hour for EC2 + storage • Lower than Oracle • Higher than RDS |
| **Redshift** | Columnar | • Analytics • Data Warehousing  | Similar to RDS | IAM, VPC, KMS, SSL (similar to RDS) | highly available, auto healing features | • Massively Parallel Query Execution • compression • scale to PBs of data | Pay per node provisioned, 10% of cost vs other warehouses |
| **ElastiCache** | Key-value | • caching, user sessions, leaderboard, distributed states, pub / sub messaging, recommendation data | Same as RDS | Same as RDS but auth through Redis Auth)  | Clustering (sharding), Multi-AZ | Sub-millisecond, in-memory, read replicas for sharding | Pay per hour based on EC2 and storage usage. |
| **DynamoDB** | Key-value & document | • no SQL with transactions • serverless development • cache • small objects | • no operations needed • auto scaling capability • serverless | through IAM policies, KMS encryption, SSL in flight | • Multi AZ • Backups | • Single digit (1-9 millisecond) • DAX for read caching • Performance doesn't degrade if usage scales | Pay per provisioned capacity and storage usage (can use auto-scaling) |
| **S3** | Key-value object store | • big objects • static files • website hosting | no operations needed | • IAM • Bucket Policies • ACL • Encryption (Server/Client) • SSL | • 99.999999999% durability / 99.99% availability • Multi AZ • Cross-region replication | • Scales to thousands of read / writes per second • Transfer acceleration with CloudFront • Multi-part for big files | Pay per • Storage usage • Network cost: bandwidth to transfer and retrieve data • Requests number |
| **Athena** | S3 query engine | • Serverless *(one time)* queries on S3 • Log analytics • Lightweight queries | no operations needed | IAM + S3 security | • managed service • uses presto engine • highly available | Queries scale based on data size | pay per query / per TB of data scanned |
| **Neptune** | Graph | High relationship data e.g. social networking, knowledge graphs (wikis) | similar to RDS | IAM, VPC, KMS, SSL (similar to RDS) + IAM Authentication | Multi-AZ, clustering with read replicas | best suited for graphs, clustering to improve performance | pay per node provisioned (similar to RDS) |
| **ElasticSearch** | Lucene search engine | • complement to another database • big data • log analysis  | similar to RDS | Cognito, IAM, VPC, KMS, SSL | Multi-AZ, clustering | based on ElasticSearch project (open source), petabyte scale | pay per node provisioned (similar to RDS) |

### RDS vs DynamoDB Availability and Scalability

| | Aurora | DynamoDB |
| - | ----- | ------- |
| Roll-back impact | No downtime, more IO (uses snapshots), requires re-establishing connection after DB change | No downtime, no IO (uses Backup and Restore), requires re-establishing connection after DB change |
| Multi-master | Across AZ in single region (even with global database) | Only across regions with global tables |
| Multi-reader | Across AZ & regions | Automatically, only can set-up with DAX |
| Failover to replicas | Any replica | Any replica |

## Aurora Vs Dynamo Vs RDS			

|   | Aurora   | DynamoDB   | RDS  | 
|---|---|---|---|
| 1  | No of Read Replica   |15   | NA   | 5 |
| 2  | AutomaticFailover Replication  |Same   | Same  | Same (Via Standby Replica| 
| 3  | Monitoring  |Same   | Same  | Same |
| 4  | Purchasing  |Same   | Same  | Same |
| 5  |  Serverless |Yes   | No  | No|
| 6  | Global Features |Yes (Global DB)   | Yes (Global Tables)  | No |
| 7  | Endpoints  |4 Endpoints   | NA  | NA |
| 8  | Caching  |Yes(Cache warming)   | DAX  | NA  | 
| 9  | TTL  |No   | Yes  | NA |
| 10 | Consistency  | Same  |  Same | Same  | 
| 11 | Encryption At Rest  | Same  |  Same | Same  |
| 12 | Point In time Recovery | Same  | Same  | Same  |



## Data migration

- **Native SQL DB migration**: From SQL Server => upload `.bak` to S3 => restore from `.bak` with SQL statement in RDS
- **AWS Database Migration service**
  - Migrate and/or replicate databases and data warehouses to AWS.
    - ***From***
      - ***On-premises/EC2/Azure***: Oracle, SQL Server, Azure SQL, PostreSQL, MySQL, SAP ASE, MongoDB, S3, DB2
      - ***AWS Native***: RDS & Aurora & S3
    - ***To***:
      - ***AWS-native***: RDS, S3, DynamoDB, Redshift, Kinesis Data Streams, Elasticsearch, DocumentDB
      - ***On-premises/EC2***: Oracle, SQL Server, PostgreSQL, MySQL, SAP ASE
  - Supports
    - **Cloud to cloud**: e.g. you can stream to Amazon Kinesis Data Streams through AWS Database Migration Service
    - **On-prem to cloud**
      - You can migrate SQL databases
        - 💡 Instead: native SQL DB migration through `.bak` file is recommended.
        - 💡 Only recommended if your database cannot be offline while back-up is created/copied and restored.
    - **On-prem to on-prem**
      - E.g. upgrade a minor version with on-prem SAP ASE to on-prem SAP ASE
  - **Schema Conversion Tool**: E.g. from NoSQL to SQL, SQL to NoSQL or NoSQL to NoSQL.
  - **Data Extractor**: Runs on-prem, pulls data from DB, uploads to S3 so other DBs can pull it from S3.
  - 💡 Other use-cases: Classic to VPC, Data warehouse to Redshift, Consolidate shards into Aurora, Archive old data, migrate from NoSQL <=> SQL or NoSQL <=> NoSQL.
  - **Pricing**
    - Ingress into AWS Database Migration Service is free
    - Egress to RDS and EC2 in same AZ is also free.
