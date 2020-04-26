> #### [Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

- fully managed relational database engine that's compatible with MySQL and PostgreSQL.
- With some workloads, Aurora can deliver up to five times the throughput of MySQL and up to three times the throughput of PostgreSQL without requiring changes to most of your existing applications.
- The underlying storage grows automatically as needed, up to **64 terabytes**.The **minimum is 10 GB**
-  is part of the managed database service Amazon Relational Database Service (Amazon RDS).

Amazon Aurora DB Clusters
- Aurora DB cluster consists of one or more DB instances and a cluster volume that manages the data for those DB instances
- An Aurora cluster volume is a virtual database storage volume that spans multiple Availability Zones, with each Availability Zone having a copy of the DB cluster data
- Two types of DB instances make up an Aurora DB cluster:
  - Primary DB instance
    - Supports read and write operations, and performs all of the data modifications to the cluster volume. Each Aurora DB cluster has one primary DB instance.
  - Aurora Replica
    - connects to the same storage volume as the primary DB instance and supports only read operations.
    - can have up to **15 Aurora Replicas** in addition to the primary DB instance.
    - Aurora automatically fails over to an Aurora Replica in case the primary DB instance becomes unavailable.
    - can specify the failover priority for Aurora Replicas
    - can also offload read workloads from the primary DB instance.

Types of Aurora Endpoints
- Cluster endpoint
  - connects to the current primary DB instance for that DB cluster.
  - is the only one that can perform write operations such as DDL statements
  - Each Aurora DB cluster has one cluster endpoint and one primary DB instance
  - cluster endpoint provides failover support for read/write connections to the DB cluster
- Reader endpoint
  - connects to one of the available Aurora Replicas
  - Each Aurora DB cluster has one reader endpoint
  - provides load-balancing support for read-only connections
  - reader endpoint for read operations, such as queries
  - If the DB cluster contains only a primary DB instance, the reader endpoint serves connection requests from the primary DB instance.
- Custom endpoint
  - represents a set of DB instances that you choose
  - provides load balancing on the instances you chose
  - can create up to **five** custom endpoints for each provisioned Aurora cluster
  - he custom endpoint provides load-balanced database connections based on criteria other than the read-only or read-write capability of the DB instances.
  - By using custom endpoints, you can avoid updating CNAME records when your cluster grows or shrinks
  -
- Instance endpoint
  - connects to a specific DB instance within an Aurora cluster.
  - Each DB instance in a DB cluster, regardless of instance type, has its own unique instance endpoint.
  - provides direct control over connections to the DB cluster, for scenarios where using the cluster endpoint or reader endpoint might not be appropriate

High Availability
- stores copies of the data in a DB cluster across multiple Availability Zones in a single AWS Region,
- specify multi-AZ deployment
- If the primary DB instance of a DB cluster fails, Aurora automatically fails over to a new primary DB instance.It does so by either:
  - promoting an existing Aurora Replica to a new primary DB instance
  - creating a new primary DB instance

Amazon Aurora Storage and Reliability
- Aurora data is stored in the cluster volume, which is a single, virtual volume that uses solid state drives (SSDs)
- A cluster volume consists of copies of the data across multiple Availability Zones in a single AWS Region.
- data is automatically replicated across Availability Zones
- amount of replication is independent of the number of DB instances in your cluster.
- Aurora shared storage architecture makes your data independent from the DB instances in the cluster.
- For example, you can add a DB instance quickly because Aurora doesn't make a new copy of the table data. Instead, the DB instance connects to the shared volume that already contains all your data.
- maximum table size for a table in an Aurora DB cluster is 64 TiB.

 Aurora Data Storage Billing
 - charged for the space that you use in an Aurora cluster volume
 - When Aurora data is removed, such as by dropping a table or partition, the overall allocated space remains the same.The free space is reused automatically when data volume increases in the future.
 - Because storage costs are based on the storage "high water mark" (the maximum amount that was allocated for the Aurora cluster at any point in time), you can manage costs by avoiding ETL practices that create large volumes of temporary information, or that load large volumes of new data prior to removing unneeded older data.
 - If removing data from an Aurora cluster results in a substantial amount of allocated but unused space, resetting the high water mark requires doing a logical data dump and restore to a new cluster, using a tool such as mysqldump

 Amazon Aurora Reliability
 - Storage Auto-Repair
  -  Aurora automatically detects failures in the disk volumes that make up the cluster volume. When a segment of a disk volume fails, Aurora immediately repairs the segment. When Aurora repairs the disk segment, it uses the data in the other volumes that make up the cluster volume to ensure that the data in the repaired segment is current. As a result, Aurora avoids data loss and reduces the need to perform a point-in-time restore to recover from a disk failure.
- Survivable Cache Warming
  - Aurora "warms" the buffer pool cache when a database starts up after it has been shut down or restarted after a failure. That is, Aurora preloads the buffer pool with the pages for known common queries that are stored in an in-memory page cache. This provides a performance gain by bypassing the need for the buffer pool to "warm up" from normal database use.
  - The Aurora page cache is managed in a separate process from the database, which allows the page cache to survive independently of the database. In the unlikely event of a database failure, the page cache remains in memory, which ensures that the buffer pool is warmed with the most current state when the database restarts.
- Crash Recovery
  - Aurora is designed to recover from a crash almost instantaneously and continue to serve your application data without the binary log. Aurora performs crash recovery asynchronously on parallel threads, so that your database is open and available immediately after a crash.
  - The following are considerations for binary logging and crash recovery on Aurora MySQL:
    - Enabling binary logging on Aurora directly affects the recovery time after a crash, because it forces the DB instance to perform binary log recovery.
    - The type of binary logging used affects the size and efficiency of logging.
    - The amount of binary log data affects recovery time
    - Aurora does not need the binary logs to replicate data within a DB cluster or to perform point in time restore (PITR).
    - If you don't need the binary log for external replication (or an external binary log stream), we recommend that you set the binlog_format parameter to OFF to disable binary logging

Amazon Aurora Security
- Same as other services

Aurora Global Database
- consists of one primary AWS Region where your data is mastered, and one read-only, secondary AWS Region
-  Aurora replicates data to the secondary AWS Region with typical latency of under a second
- uses dedicated infrastructure to replicate your data, leaving database resources available entirely to serve application workloads
- n the unlikely event your database becomes degraded or isolated in an AWS region, you can promote the secondary AWS Region to take full read-write workloads in under a minute.
- replication performed by an Aurora global database has limited to no performance impact on the primary DB cluster.
- can add up to 16 Aurora Replicas to the secondary cluster,
- Global Database is only available for Aurora with MySQL 5.6 compatibility.
- Aurora Global Database isn't available in the EU (Stockholm) and Asia Pacific (Hong Kong) regions.
- secondary cluster must be in a different AWS Region than the primary cluster.
- can't create a cross-region Read Replica from the primary cluster in same region as the secondary.
- Some of the features like Cloning,Backtrack,Parallel query,Aurora Serverless, Stopping and starting the DB clusters within the global database are not supported by global db

Amazon Aurora Serverless
- is an on-demand, autoscaling configuration for Amazon Aurora
- An Aurora Serverless DB cluster is a DB cluster that automatically starts up, shuts down, and scales up or down capacity based on your application's needs
- A non-Serverless DB cluster for Aurora is called a provisioned DB cluster.
- features  
  - Simpler, Scalable, Cost-effective, Highly available storage
- Use Cases
  - Infrequently used applications : used for a few minutes several times per day or week, such as a low-volume blog site
  - New applications : unsure about which instance size you need
  - Variable workloads : running a lightly used application, with peaks of 30 minutes to several hours a few times each day, or several times per year.
  - Unpredictable workloads : running workloads where there is database usage throughout the day, but also peaks of activity that are hard to predict.
  - Development and test databases
  - Multi-tenant applications
- Limitations
  - only available for Aurora with MySQL 5.6 compatibility
  - port number for connections must be 3306
  - can't give an Aurora Serverless DB cluster a public IP address
  - You can access an Aurora Serverless DB cluster only from within a virtual private cloud (VPC) based on the Amazon VPC service.
  - DB subnet group used by Aurora Serverless can’t have more than one subnet in the same Availability Zone
  - Doesn't support many features which provisioned db cluster does

Pricing
- Amazon RDS instances in an Aurora cluster are billed based on the following components:
  - DB instance hours (per hour)
  - I/O requests (per 1 million requests per month)
  - Backup storage (per GiB per month)
  - Data transfer (per GB)
- Amazon RDS provides the following purchasing options
  - On-Demand Instances : Pay by the hour for the DB instance hours that you use.
  - Reserved Instances : Reserve a DB instance for a one-year or three-year term and get a significant discount compared to the on-demand DB instance pricing

Monitoring Amazon RDS
- Automated Monitoring Tools
  - Amazon RDS Events – Subscribe to Amazon RDS events to be notified when changes occur with a DB instance, DB cluster, DB cluster snapshot, DB parameter group, or DB security group
  - Database log files – View, download, or watch database log files using the Amazon RDS console or Amazon RDS API actions. You can also query some database log files that are loaded into database tables
  - Amazon RDS Enhanced Monitoring — Look at metrics in real time for the operating system
  - Amazon CloudWatch Metrics – Amazon RDS automatically sends metrics to CloudWatch every minute for each active database. You are not charged additionally for Amazon RDS metrics in CloudWatch.
  - Amazon CloudWatch Alarms – You can watch a single Amazon RDS metric over a specific time period, and perform one or more actions based on the value of the metric relative to a threshold you set
  - Amazon CloudWatch Logs – Most DB engines enable you to monitor, store, and access your database log files in CloudWatch Logs.
- Manual Monitoring Tools
  - Amazon RDS console
  - AWS Trusted Advisor dashboard
  - CloudWatch home page

Differences Between CloudWatch and Enhanced Monitoring Metrics
- CloudWatch gathers metrics about CPU utilization from the hypervisor for a DB instance, and Enhanced Monitoring gathers its metrics from an agent on the instance. As a result, you might find differences between the measurements, because the hypervisor layer performs a small amount of work. The differences can be greater if your DB instances use smaller instance classes, because then there are likely more virtual machines (VMs) that are managed by the hypervisor layer on a single physical instance. Enhanced Monitoring metrics are useful when you want to see how different processes or threads on a DB instance use the CPU.


Other points
- Aurora is AWS Cloud optimized
- Master + up to 15 read replicas
- failover instantaneous . HA native
- costs more than RDS ( 20 %)
- replication + self- healing + Auto expanding ( shared storage 10gb to 64gb)
- supports cross region replication
- read replicatas -> LBed + auto scaling
- Encryption at rest using KMS
- Encryption in-flight using SSL
- Authentication using IAM
- you are responsibel for protecting the instance with security groups
- you can't ssh to db instances
