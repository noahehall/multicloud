# neptune

- fully managed serverless graph database for highly connected, multi-layered datasets

## my thoughts

- since it uses tinkerpop, i'm presuming we can migrate data out of neptune into something else if push comes to shove

## links

- [landing page](https://aws.amazon.com/neptune/?did=ap_card&trk=ap_card)
- [running local](https://docs.aws.amazon.com/neptune/latest/userguide/graph-notebooks.html)
- [getting started](https://docs.aws.amazon.com/neptune/latest/userguide/get-started.html)
- [bulk load tutorial](https://docs.aws.amazon.com/neptune/latest/userguide/bulk-load-tutorial-IAM.html)
- [bulk load user guide](https://docs.aws.amazon.com/neptune/latest/userguide/bulk-load.html)
- [db clusters](https://docs.aws.amazon.com/neptune/latest/userguide/feature-overview-db-clusters.html)
- [storage](https://docs.aws.amazon.com/neptune/latest/userguide/feature-overview-storage.html)
- [security](https://docs.aws.amazon.com/neptune/latest/userguide/security.html)
- [onramp to graph dbs & neptune](https://www.slideshare.net/AmazonWebServices/onramp-to-graph-databases-and-amazon-neptune-dat335-aws-reinvent-2018?qid=e677d773-0cb1-452d-95a3-b0be4d1dc7d9)

## best practices

### anti patterns

## features

- read replicas for highly availability
- create point-in-time copies, configure continuous backup to Amazon Simple Storage Service (Amazon S3) with replication across Availability Zones
- supports two popular graph query languages: Apache TinkerPop and RDF/SPARQL
- a cloud-native storage service that provides high-availability support using multiple Availability Zones for up to 15 read replicas and support for encryption at rest
- quorum system for read/write

### pricing

- instance hosting: on demand by hour
- storage consumed: per gigabyte per month
- requests in
- data out

## terms

## basics

### architecture

- primary database: read and write operations and performs all the data modifications to the cluster volume
- read-only replicas: up to 15; connects to the same storage volume as the primary database instance
- cluster volume: esigned for reliability and high availability; copies data across AZs in a single region.
- stores three fields for each connection or relationship
- endpoints:
  - cluster: connects to the current primary database instance for the database cluster.
  - reader: connects to one of the available Neptune replicas. Each replica has its own endpoint
  - instance: connects to a specific database instance; provides direct control over connections to the DB cluster, for scenarios where using the cluster endpoint or reader endpoint might not be appropriate
- availability:
  - failing db nodes auto detected and replaced
  - failing db processes auto detected and recycled
  - replicas auto promoted to primary during failover
  - customer-specific failover order
- performance
  - scale out read traffic across read replicas
  - reader endpoint balances connections across read replicas

### Graph Data Models

- each model has its own query language

#### Property Graph

- Apache tinkerpop
- gremlin traversal language

#### Resource Description Framework (RDF)

- w3c standard
- SPARQL query language

### security

- network isolation via VPC
- control ingress via security groups

### encryption

- data at rest in the database is encrypted using industry standard AES-256 KMS
  - Keys can also be used, which are managed through KMS

## considerations

- db engine version
- instance class
- high availability: can be disabled
- db instance identifier: must be unique per account per region
- vpc, subnets, security group
- db cluster identifier, db port (e.g. 8192), parameter group
- iam authnz, encryption at rest: both can be disabled/enabled at creation
- failover
- backup retention period & window
- version maintenance and window

## integrations

### IAM

- user authn at creation
- roles can be assigned at anytime (e.g. to load data from s3)

### kms

### kinesis

### lambda

### VPC

- requires atleast two subnets in two different Availability Zones for high availability
  - dude stop spamming this everywhere, this is a default for high availability

### s3

### cloudtrail

### SNS
