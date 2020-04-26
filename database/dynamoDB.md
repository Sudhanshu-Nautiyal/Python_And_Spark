> #### [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

- Amazon DynamoDB is a fully managed **NoSQL database** service that provides fast and predictable performance with seamless scalability. You can use Amazon DynamoDB to create a database table that can store and retrieve any amount of data, and serve any level of request traffic. Amazon DynamoDB automatically spreads the data and traffic for the table over a sufficient number of servers to handle the request capacity specified by the customer and the amount of data stored, while maintaining consistent and fast performance.
- offers encryption at rest
-  store and retrieve any amount of data, and serve any level of request traffic
- provides on-demand backup capability.
- provides point-in-time recovery for your Amazon DynamoDB tables
- TTL - allows you to delete expired items from tables automatically to help you reduce storage usage and the cost of storing data that is no longer relevant.
- All of your data is stored on solid state disks (SSDs) and automatically replicated across multiple Availability Zones in an AWS region
- can use global tables to keep DynamoDB tables in sync across AWS Regions
- DynamoDB is a NoSQL database and is schemaless. This means that, other than the primary key attributes, you don't have to define any attributes or data types when you create tables.

DynamoDB Core Components

The following are the basic DynamoDB components:

**Tables** – Similar to other database systems, DynamoDB stores data in tables. A table is a collection of data. For example, see the example table called People that you could use to store personal contact information about friends, family, or anyone else of interest. You could also have a Cars table to store information about vehicles that people drive.

**Items** – Each table contains zero or more items. An item is a group of attributes that is uniquely identifiable among all of the other items. In a People table, each item represents a person. For a Cars table, each item represents one vehicle. Items in DynamoDB are similar in many ways to rows, records, or tuples in other database systems. In DynamoDB, there is no limit to the number of items you can store in a table.

**Attributes** – Each item is composed of one or more attributes. An attribute is a fundamental data element, something that does not need to be broken down any further. For example, an item in a People table contains attributes called PersonID, LastName, FirstName, and so on. For a Department table, an item might have attributes such as DepartmentID, Name, Manager, and so on. Attributes in DynamoDB are similar in many ways to fields or columns in other database systems.


Primary Key

DynamoDB supports two different kinds of primary keys:
- Partition key
  - A simple primary key, composed of one attribute known as the partition key.
  - DynamoDB uses the partition key's value as input to an internal hash function. The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored.
  - In a table that has only a partition key, no two items can have the same partition key value.
  - partition key of an item is also known as its hash attribute
  - Each primary key attribute must be a scalar (meaning that it can hold only a single value)
- Partition key and sort key (composite primary key)
  - this type of key is composed of two attributes
  - first attribute is the partition key
  - second attribute is the sort key.
  - DynamoDB uses the partition key value as input to an internal hash function. The output from the hash function determines the partition (physical storage internal to DynamoDB) in which the item will be stored.
  - All items with the same partition key value are stored together, in sorted order by sort key value.
  - In a table that has a partition key and a sort key, it's possible for two items to have the same partition key value. However, those two items must have different sort key values.
  - sort key of an item is also known as its range attribute

Secondary Indexes
- A secondary index lets you query the data in the table using an alternate key, in addition to queries against the primary key
- You can create one or more secondary indexes on a table
- DynamoDB supports two kinds of indexes:
  - Global secondary index – An index with a partition key and sort key that can be different from those on the table.
  - Local secondary index – An index that has the same partition key as the table, but a different sort key.
- Each table in DynamoDB has a limit of **20** global secondary indexes (default limit) and **5** local secondary indexes per table.

DynamoDB Streams
- optional feature that captures data modification events in DynamoDB tables
- The data about these events appear in the stream in near real time, and in the order that the events occurred.
- Each event is represented by a stream record. If you enable a stream on a table, DynamoDB Streams writes a stream record whenever one of the following events occurs:
  - A new item is added to the table - The stream captures an image of the entire item, including all of its attributes.
  - An item is updated - The stream captures the "before" and "after" image of any attributes that were modified in the item.
  - An item is deleted from the table - The stream captures an image of the entire item before it was deleted.
- Each stream record also contains the name of the table, the event timestamp, and other metadata
- Stream records have a lifetime of 24 hours; after that, they are automatically removed from the stream.
- You can use DynamoDB Streams together with AWS Lambda to create a trigger—code that executes automatically whenever an event of interest appears in a stream

Data Types
- DynamoDB supports many different data types for attributes within a table. They can be categorized as follows:
  - Scalar Types – A scalar type can represent exactly one value. The scalar types are number, string, binary, Boolean, and null.
  - Document Types – A document type can represent a complex structure with nested attributes—such as you would find in a JSON document. The document types are list and map.
  - Set Types – A set type can represent multiple scalar values.
   The set types are string set, number set, and binary set.

Read Consistency
-  Each region is independent and isolated from other AWS regions.For example, if you have a table called People in the us-east-2 region and another table named People in the us-west-2 region, these are considered two entirely separate tables
- DynamoDB supports eventually consistent and strongly consistent reads.
- Eventually Consistent Reads
  - When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.
- Strongly Consistent Reads
  - When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. A strongly consistent read might not be available if there is a network delay or outage
  - Consistent reads are not supported on global secondary indexes (GSI).
  - DynamoDB uses eventually consistent reads, unless you specify otherwise.
  - Read operations (such as GetItem, Query, and Scan) provide a ConsistentRead parameter. If you set this parameter to true, DynamoDB uses strongly consistent reads during the operation.

Read/Write Capacity Mode
- Amazon DynamoDB has two read/write capacity modes for processing reads and writes on your tables:
  - On-demand
    - flexible billing option capable of serving thousands of requests per second without capacity planning.
    -  offers pay-per-request pricing for read and write requests so that you pay only for what you use.
    - On-demand mode is a good option if any of the following are true:
      - You create new tables with unknown workloads.
      - You have unpredictable application traffic.
      - You prefer the ease of paying for only what you use.
    - DynamoDB charges you for the reads and writes that your application performs on your tables in terms of read request units and write request units.
    - Read request unit  
      - One read request unit represents one strongly consistent read request, or two eventually consistent read requests, for an item up to 4 KB in size
      - Transactional read requests require 2 read request units to perform one read for items up to 4 KB.
      - The total number of read request units required depends on the item size, and whether you want an eventually consistent or strongly consistent read. For example, if your item size is 8 KB, you require 2 read request units to sustain one strongly consistent read, 1 read request unit if you choose eventually consistent reads, or 4 read request units for a transactional read request.
    - Write request unit
      - One write request unit represents one write for an item up to 1 KB in size
      - Transactional write requests require 2 write request units to perform one write for items up to 1 KB
      -  The total number of write request units required depends on the item size. For example, if your item size is 2 KB, you require 2 write request units to sustain one write request or 4 write request units for a transactional write request.
    - Table Behavior while Switching Read/Write Capacity - ModeWhen you switch a table from provisioned capacity mode to on-demand capacity mode, DynamoDB makes several changes to the structure of your table and partitions. This process can take several minutes. During the switching period, your table delivers throughput that is consistent with the previously provisioned write capacity unit and read capacity unit amounts. When switching from on-demand capacity mode back to provisioned capacity mode, your table delivers throughput consistent with the previous peak reached when the table was set to on-demand capacity mode.
  - Provisioned Mode
    - If you choose provisioned mode, you specify the number of reads and writes per second that you require for your application
    - u can use auto scaling to adjust your table’s provisioned capacity automatically in response to traffic changes
    - Provisioned mode is a good option if any of the following are true:
      - You have predictable application traffic.
      - You run applications whose traffic is consistent or ramps gradually.
      - You can forecast capacity requirements to control costs.
      - Read Capacity Units and Write Capacity Units - same
      - For example, suppose that you create a provisioned table with 6 read capacity units and 6 write capacity units. With these settings, your application could do the following:
        - Perform strongly consistent reads of up to 24 KB per second (4 KB × 6 read capacity units).
        - Perform eventually consistent reads of up to 48 KB per second (twice as much read throughput).
        - Perform transactional read requests of up to 3 KB per second.
        - Write up to 6 KB per second (1 KB × 6 write capacity units).
        - Perform transactional write requests of up to 3 KB per second
    - Provisioned throughput is the maximum amount of capacity that an application can consume from a table or index.
    - If your application exceeds your provisioned throughput capacity on a table or index, it is subject to request throttling.
    - Throttling prevents your application from consuming too many capacity units. When a request is throttled, it fails with an HTTP 400 code (Bad Request) and a ProvisionedThroughputExceededException.

DynamoDB Auto Scaling
- DynamoDB auto scaling actively manages throughput capacity for tables and global secondary indexes. With auto scaling, you define a range (upper and lower limits) for read and write capacity units. You also define a target utilization percentage within that range. DynamoDB auto scaling seeks to maintain your target utilization, even as your application workload increases or decreases.
- With DynamoDB auto scaling, a table or a global secondary index can increase its provisioned read and write capacity to handle sudden increases in traffic, without request throttling. When the workload decreases, DynamoDB auto scaling can decrease the throughput so that you don't pay for unused provisioned capacity.

Reserved Capacity
- With reserved capacity, you pay a one-time upfront fee and commit to a minimum usage level over a period of time. By reserving your read and write capacity units ahead of time, you realize significant cost savings compared to on-demand provisioned throughput settings.
- Reserved capacity is not available in on-demand mode.


Point-in-Time Recovery for DynamoDB
- Point-in-time recovery helps protect your Amazon DynamoDB tables from accidental write or delete operations.
- With point-in-time recovery, you can restore that table to any point in time during the last 35 days.
- point-in-time operations don't affect performance or API latencies.
- The point-in-time recovery process always restores to a new table.
- If you disable point-in-time recovery and later re-enable it on a table, you reset the start time for which you can recover that table. As a result, you can only immediately restore that table using the LatestRestorableDateTime.
- Along with data, the following are also included on the new restored table using point in time recovery:
  - Global secondary indexes (GSIs)
  - Local secondary indexes (LSIs)
  - Provisioned read and write capacity
  - Encryption settings

Amazon DynamoDB Encryption at Rest
- All user data stored in Amazon DynamoDB is fully encrypted at rest using encryption keys stored in AWS Key Management Service (AWS KMS).
- When creating a new table, you can choose one of the following customer master keys (CMK) to encrypt your table
- DynamoDB encryption at rest provides an additional layer of data protection by securing your data in the encrypted table, including its primary key, local and global secondary indexes, streams, global tables, backups, and DynamoDB Accelerator (DAX) clusters whenever the data is stored in durable media.
- Encryption at rest using the AWS owned CMK is offered at no additional cost

Amazon DynamoDB Transactions
- simplify the developer experience of making coordinated, all-or-nothing changes to multiple items both within and across tables.
- Transactions provide atomicity, consistency, isolation, and durability (ACID) in DynamoDB
- no additional cost to enable transactions for your DynamoDB tables
- Transactional operations are disabled for global tables by default. Contact Amazon Support if you want to enable transactions on global tables.
- DynamoDB transactions provide a more cost-effective, robust, and performant replacement for the AWSLabs transactions client library.

In-Memory Acceleration with DAX
-  In most cases, the DynamoDB response times can be measured in single-digit milliseconds. However, there are certain use cases that require response times in microseconds.
- For these use cases, DynamoDB Accelerator (DAX) delivers fast response times for accessing eventually consistent data.
- DAX is a DynamoDB-compatible caching service that enables you to benefit from fast in-memory performance for demanding applications. DAX addresses three core scenarios:
  - As an in-memory cache, DAX reduces the response times of eventually-consistent read workloads by an order of magnitude, from single-digit milliseconds to microseconds.
  - DAX reduces operational and application complexity by providing a managed service that is API-compatible with Amazon DynamoDB, and thus requires only minimal functional changes to use with an existing application.
  - For read-heavy or bursty workloads, DAX provides increased throughput and potential operational cost savings by reducing the need to over-provision read capacity units. This is especially beneficial for applications that require repeated reads for individual keys.
- DAX supports server-side encryption. With encryption at rest, the data persisted by DAX on disk will be encrypted. DAX writes data to disk as part of propagating changes from the primary node to read replicas.
- Use Cases for DAX
  - Applications that require the fastest possible response time for reads.
  - Applications that read a small number of items more frequently than others
  - Applications that are read-intensive, but are also cost-sensitive
  - Applications that require repeated reads against a large set of data
- DAX is not ideal for:
  - Applications that require strongly consistent reads (or cannot tolerate eventually consistent reads)
  - Applications that do not require microsecond response times for reads, or that do not need to offload repeated read activity from underlying tables.
  - Applications that are write-intensive, or that do not perform much read activity.
  - Applications that are already using a different caching solution with DynamoDB, and are using their own client-side logic for working with that caching solution.


SQL or NoSQL


| Characteristic | Relational Database Management System \(RDBMS\) | Amazon DynamoDB |
| --- | --- | --- |
| Optimal Workloads | Ad hoc queries; data warehousing; OLAP \(online analytical processing\)\. | Web\-scale applications, including social networks, gaming, media sharing, and IoT \(Internet of Things\)\. |
| Data Model | The relational model requires a well\-defined schema, where data is normalized into tables, rows and columns\. In addition, all of the relationships are defined among tables, columns, indexes, and other database elements\. | DynamoDB is schemaless\. Every table must have a primary key to uniquely identify each data item, but there are no similar constraints on other non\-key attributes\. DynamoDB can manage structured or semi\-structured data, including JSON documents\. |
| Data Access | SQL \(Structured Query Language\) is the standard for storing and retrieving data\. Relational databases offer a rich set of tools for simplifying the development of database\-driven applications, but all of these tools use SQL\. | You can use the AWS Management Console or the AWS CLI to work with DynamoDB and perform ad hoc tasks\. Applications can leverage the AWS software development kits \(SDKs\) to work with DynamoDB using object\-based, document\-centric, or low\-level interfaces\. |
| Performance | Relational databases are optimized for storage, so performance generally depends on the disk subsystem\. Developers and database administrators must optimize queries, indexes, and table structures in order to achieve peak performance\. | DynamoDB is optimized for compute, so performance is mainly a function of the underlying hardware and network latency\. As a managed service, DynamoDB insulates you and your applications from these implementation details, so that you can focus on designing and building robust, high\-performance applications\. |
| Scaling | It is easiest to scale up with faster hardware\. It is also possible for database tables to span across multiple hosts in a distributed system, but this requires additional investment\. Relational databases have maximum sizes for the number and size of files, which imposes upper limits on scalability\. | DynamoDB is designed to scale out using distributed clusters of hardware\. This design allows increased throughput without increased latency\. Customers specify their throughput requirements, and DynamoDB allocates sufficient resources to meet those requirements\. There are no upper limits on the number of items per table, nor the total size of that table\. |
