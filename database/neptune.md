> #### [Neptune](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)
Amazon Neptune is a fast, reliable, fully managed graph database service that makes it easy to build and run applications that work with highly connected datasets. The core of Neptune is a purpose-built, high-performance graph database engine that is optimized for storing billions of relationships and querying the graph with milliseconds latency. Neptune supports the popular graph query languages Apache TinkerPop Gremlin and W3C’s SPARQL, allowing you to build queries that efficiently navigate highly connected datasets. Neptune powers graph use cases such as recommendation engines, fraud detection, knowledge graphs, drug discovery, and network security.

- Use cases
  - High relationship data
  - social n/w
  - knowledge graphs (wikipedia)
- highly available across 3 AZ, with upto 15 read replicas
- Point-in-time recovery , continous backup to S3
- Support for KMS encryption at rest + HTTPS


- 5 pillars
  - Operations: similar to RDS
  - Security : IAM,VPC,KMS,SSL+IAM Auth
  - Reliability : Multi-AZ, clustering
  - Performance: suitable for graphs,clustering to improve performance
  - Cost: pay per node provisioned (similar to RDS)
