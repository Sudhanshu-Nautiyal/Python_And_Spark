> #### [RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)

- is a web service that makes it easier to set up, operate, and scale a relational database in the cloud
- Amazon RDS manages
  - backups
  - software patching
  - automatic failure detection and recovery
- To deliver a managed service experience, Amazon RDS doesn't provide shell access to DB instances, and it restricts access to certain system procedures and tables that require advanced privileges.
- high availability with a primary instance and a synchronous secondary instance that you can fail over to when problems occur
- Supported DB
  - MySQL
  - MariaDB
  - PostgreSQL
  - Microsoft SQL Server
- You can run your DB instance in several Availability Zones, an option called a Multi-AZ deployment
- Use Enhanced Monitoring to Identify Operating System Issues
- Amazon RDS provides high availability and failover support for DB instances using Multi-AZ deployments.
- The high-availability feature is not a scaling solution for read-only scenarios; you cannot use a standby replica to serve read traffic. To service read-only traffic, you should use a Read Replica
- Supports cross-region replication

DB Instances
- an isolated database environment in the cloud.
- can contain multiple user-created databases
- Each DB instance runs a DB engine
- computation and memory capacity of a DB instance is determined by its DB instance class
- DB instance storage
  - Magnetic
  - General Purpose (SSD)
  - Provisioned IOPS (PIOPS)

Failover
-  primary DB instance switches over automatically to the standby replica if any of the following conditions occur:
  - An Availability Zone outage
  - The primary DB instance fails
  - The DB instance's server type is changed
  - the operating system of the DB instance is undergoing software patching
  - A manual failover of the DB instance was initiated using Reboot with failover

RDS Read Replicas for read scalability
- Amazon RDS uses the MariaDB, MySQL, Oracle, and PostgreSQL DB engines' built-in replication functionality to create a special type of DB instance called a Read Replica from a source DB instance
- Updates made to the source DB instance are asynchronously copied to the Read Replica
- reduce the load on your source DB instance by routing read queries from your applications to the Read Replica
- Using Read Replicas, you can elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads.
- supported by MariaDB, MySQL, Oracle, and PostgreSQL engines
- Use cases
  - Scaling beyond the compute or I/O capacity of a single DB instance for read-heavy database workloads
  - Serving read traffic while the source DB instance is unavailable.
  - Business reporting or data warehousing scenarios where you might want business reporting queries to run against a Read Replica, rather than your primary, production DB instance.
  - Implementing disaster recovery. You can promote a Read Replica to a standalone instance as a disaster recovery solution if the source DB instance fails.
- Up to 5 Read Replicas
- Within AZ, Cross AZ Application or Cross Region
- Replication is ASYNC, so reads are eventually consistent
- Replicas can be promoted to their own DB
- Applications must update the connection string to leverage read replicas

Promoting a Read Replica to Be a Standalone DB Instance
- reasons you might want to promote a Read Replica to a standalone DB instance:
  - Performing DDL operations (MySQL and MariaDB only)
  - Sharding
  - Implementing failure recovery

RDS Backups
- Backups are automatically enabled in RDS
- Automated backups:
  - Daily full snapshot of the database
  - Capture transaction logs in real time
  - ability to restore to any point in time
  - 7 days retention (can be increased to 35 days)

DB Snapshots
- Manually triggered by the user
- Retention of backup for as long as you want

RDS Encryption
- Encryption at rest capability with AWS KMS - AES-256 encryption
- SSL certificates to encrypt data to RDS in flight
- To enforce SSL:
  - PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (Paratemer Groups)
  - MySQL: Within the DB:GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;
- To connect using SSL:
  - Provide the SSLTrust certificate (can be download from AWS)
  - Provide SSL options when connecting to database


Option Groups
 - additional db specific features

DB Parameter Groups
- manage your DB engine configuration by associating your DB instances with parameter groups.
- A DB parameter group acts as a container for engine configuration values that are applied to one or more DB instances
- If you create a DB instance without specifying a DB parameter group, the DB instance uses a default DB parameter group

Storage
- can't reduce the amount of storage for a DB instance after it has been allocated.
- can change the type of storage for your DB instance

Purchasing options
- On-Demand Instances
- Reserved Instances

Backup and restoring
- Supports
  - automatic backups
  - manual backup
- You can't restore a DB instance from a DB snapshot that is both shared and encrypted. Instead, you can make a copy of the DB snapshot and restore the DB instance from the copy.
- If you copy an encrypted snapshot, the copy of the snapshot must also be encrypted
- If you copy an encrypted snapshot across regions, you can't use the same KMS encryption key for the copy as used for the source snapshot, because KMS keys are region-specific.
- You can also encrypt a copy of an unencrypted snapshot
- When you copy a snapshot to an AWS Region that is different from the source snapshot's AWS Region, the first copy is a full snapshot copy, even if you copy an incremental snapshot. A full snapshot copy contains all of the data and metadata required to restore the DB instance. After the first snapshot copy, you can copy incremental snapshots of the same DB instance to the same destination region within the same AWS account.
- To share an automated DB snapshot, create a manual DB snapshot by copying the automated snapshot, and then share that copy.
- You can restore a DB instance to a specific point in time, creating a new DB instance

Monitoring
- Automated Monitoring Tools
  - Amazon RDS Events - notified when changes occur
  - Database log files
  - Amazon RDS Enhanced Monitoring - real time for os metrics
  - Amazon CloudWatch Metrics
  - Amazon CloudWatch Alarms
  - Amazon CloudWatch Logs  

Differences Between CloudWatch and Enhanced Monitoring Metrics
- CloudWatch gathers metrics about CPU utilization from the hypervisor for a DB instance, and Enhanced Monitoring gathers its metrics from an agent on the instance

Configuring Security
- Authentication and Access Control
  - Using Identity-Based Policies (IAM Policies)
  - IAM Database Authentication for MySQL and PostgreSQL
    - With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.
    -  works with MySQL and PostgreSQL  
  - Encrypting Amazon RDS Resources
  - Using SSL to Encrypt a Connection
  - Controlling Access with Security Groups
    - DB Security Groups on EC2-Classic
  - Master User Account Privileges
  - Service-Linked Roles
  - Using Amazon RDS with Amazon VPC


Other important points

- Amazon RDS supports Transparent Data Encryption (TDE) for DB encryption:
  - Oracle or SQL Server DB instance only
  - TDE can be used on top of KMS - may effect performance
- also supports IAM Authentication ( versus traditional username/pwd)
  - workds for MySQL and PostgresSQL
  - Lifespan of authenticaiton token is 15 mins(short-lived)
  - Tokens are generated by AWS credentials
  - SSL mush be used when connecting to DB
  - Easy to use EC2 instance roles to connect to the RDS DB
- You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MySQL and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.
 
 
Miscellaneous Points 

 
1. You can have up to 40 Amazon RDS DB instances per account :	
2. During backup user experience elevated latency.
3. Restore version will be a new RDS instance always with new DNS endpoint.
4. RR can be promoted to have their own DB but it breaks the replication.   
5. RDS encypted-> Read replicas, underlying storage, automated backups, snapshots too are encrypted.
6. Readreplicas have Asynchronous replication of Primary RDS instance, and each RR had its own DNS end point
