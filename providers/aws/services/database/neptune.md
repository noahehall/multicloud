# neptune

- fully managed serverless graph database for highly connected, multi-layered datasets
- [property graph](./neptune-propertyGraph.md)
- [RDF graph](./neptune-rdfGraph.md)
- bookmark
  - [architecture](https://github.com/aws-samples/aws-dbs-refarch-graph/tree/master/src/connecting-using-a-load-balancer)
  - [blah start from the top](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html)

## my thoughts

## links

- [landing page](https://aws.amazon.com/neptune/?did=ap_card&trk=ap_card)
- [running local](https://docs.aws.amazon.com/neptune/latest/userguide/graph-notebooks.html)
- [getting started](https://docs.aws.amazon.com/neptune/latest/userguide/get-started.html)
- [bulk load tutorial](https://docs.aws.amazon.com/neptune/latest/userguide/bulk-load-tutorial-IAM.html)
- [bulk load user guide](https://docs.aws.amazon.com/neptune/latest/userguide/bulk-load.html)
- [db clusters](https://docs.aws.amazon.com/neptune/latest/userguide/feature-overview-db-clusters.html)
- [storage](https://docs.aws.amazon.com/neptune/latest/userguide/feature-overview-storage.html)
- [security](https://docs.aws.amazon.com/neptune/latest/userguide/security.html)
- [slideshare: onramp to graph dbs & neptune](https://www.slideshare.net/AmazonWebServices/onramp-to-graph-databases-and-amazon-neptune-dat335-aws-reinvent-2018?qid=e677d773-0cb1-452d-95a3-b0be4d1dc7d9)
- [slideshare: graph data model and queries with neptune](https://www.slideshare.net/AmazonWebServices/work-backwards-to-your-graph-data-model-queries-with-amazon-neptune-dat330-aws-reinvent-2018)
- [slideshare: migrating to neptune](https://www.slideshare.net/AmazonWebServices/migrating-to-amazon-neptune-dat338-aws-reinvent-2018?qid=54cd934d-a746-48de-97a4-84321f6250f8)
- [best practices](https://docs.aws.amazon.com/neptune/latest/userguide/best-practices.html)
- [getting started: 7 videos 9 hrs](https://pages.awscloud.com/AWS-Learning-Path-Getting-Started-with-Amazon-Neptune_2020_LP_0009-DAT.html)
- [skillbuilder course](https://explore.skillbuilder.aws/learn/course/internal/view/elearning/14165/getting-started-with-amazon-neptune)
- [tagging resources](https://docs.aws.amazon.com/neptune/latest/userguide/tagging.html)
- [instance status checks](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-status.html)
- [audit logs](https://docs.aws.amazon.com/neptune/latest/userguide/auditing.html)
- [events](https://docs.aws.amazon.com/neptune/latest/userguide/events.html)
- [user guide](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html)
- [reference architecture](https://github.com/aws-samples/aws-dbs-refarch-graph)
- [tools and utilities](https://github.com/awslabs/amazon-neptune-tools)

### opensource

- [convert graphML to neptune](https://github.com/awslabs/amazon-neptune-tools/blob/master/graphml2csv/README.md)
- [open cycpher](https://opencypher.org/)
- [apache tinkerpop](https://tinkerpop.apache.org/)
- [w3c RDF](https://www.w3.org/RDF/)
- [w3c SPARQL](https://www.w3.org/TR/sparql11-query/)

## best practices

- read replicas should always be equal to or larger than the writer instance
  - avoids the larger writer instance from handling changes too quickly for the reader to maintain pace.

### anti patterns

- avoid creating ID properties

## features

- read replicas for highly availability
- create point-in-time copies, configure continuous backup to Amazon Simple Storage Service (Amazon S3) with replication across Availability Zones
- supports two popular graph query languages: Apache TinkerPop and RDF/SPARQL
- a cloud-native storage service that provides high-availability support using multiple Availability Zones for up to 15 read replicas and support for encryption at rest
- quorum system for read/write

### pricing

- pay for the compute, I/O, and storage costs that your workload requires.
  - ballpark: costs break down to about 85 percent EC2 instances, 10 percent storage, and 5 percent I/O, but costs vary based on the workload.
- instance hosting: on demand by hour
  - primary instances used for read-write workloads
  - read replicas used to scale reads and enhance failover
  - Neptune workbench instances: work with your Neptune cluster using Jupyter notebooks (hosted by Amazon SageMaker).
- storage consumed: per gigabyte per month
  - automated database backups and customer-initiated database cluster snapshots
- i/o: billed in per million request increments
  - requests in
  - data out

## terms

## basics

- availability:
  - failing db nodes auto detected and replaced
  - failing db processes auto detected and recycled
  - replicas auto promoted to primary during failover
  - customer-specific failover order
- performance
  - scale out read traffic across read replicas
  - reader endpoint balances connections across read replicas

### Data model

- neptune will create UUIDs for new verticies & edges
  - you can still supply your own unique IDs
    - required when bulk uploading
- labels: are tags, keyless properties
  - generally used to type a vertex, or name a relationship
  - verticies must have one/more labels
  - edges must have exactly one label

### Performance

- directly related to how much of the graph a query must touch
  - choose domain-meaningful edge labels
  - discover only what is absolutely necessary

### cluster

- primary database: read and write operations and performs all the data modifications to the cluster volume
- compute layer
  - single-writer architecture: no more than a single writer instance can be provisioned.
  - read replicas: up to 15; connects to the same storage volume as the primary database instance
    - Readsare eventually consistent: generally in the single-digit milliseconds, but potentially up to 100 ms under extremely heavy traffic.
- storage layer: spans multiple AZs
  - cluster volume: designed for reliability and high availability; copies data across AZs in a single region.

#### endpoints:

- cluster: connects to the current primary database instance for the database cluster.
- instance: connects to a specific database instance; provides direct control over connections to the DB cluster, for scenarios where using the cluster endpoint or reader endpoint might not be appropriate
- writer: points at the writer instance and should be used for any query that modifies data in the cluster.
- reader: rotates distribution of requests across all of the read replicas in the cluster.
  - connects to one of the available Neptune replicas. Each replica has its own endpoint

#### Parameter Groups

- db parameter groups: apply at the instance level
- db cluster parameter groups: apply to every instance in the cluster
- default parameter & cluster parameter groups
  - every account has them, cant be modified
- customer paramter and cluster parameter groups
  - all Neptune DB parameters are static
  - you must manually reboot each db instance before changes take effect

#### Instances

- DB instances are created by default with a firewall and a default security group that prevents access
  - To enable access, you must have a VPC security group with additional rules.
- choosing the right class:
  - number of concurrent requests
  - write latency
  - query complexity
  - query performance requirements
  - etc
- testing query performance:
  - optimal concurrency for writing or querying data is twice the number of vCPUs:
    - e.g. db.r5.large instance has TWO vCPUs -> FOUR processes writing data in parallel to test load times

### Notebooks

- jupyter notebooks: run python code

### Audit Logs

- View, download, or watch database log files using the Neptune console.

### Events

- Subscribe to Neptune events within the neptune console

### Graph Data Models

- each model has its own query language
- property vs RDF
  - property: your workload requires applying computations over edge attributes in the course of a graph traversal, or model bi-directional relationships
    - no schema and no predefined vocabularies for property names and labels; you must create and enforce constraints around naming of labels and such
    - support edge properties, making it easy to associate edge attributes with the edge definition
    - allows you to create multiple disconnected subgraphs within the same dataset, but has no equivalent to named graphs that allows you to identify and address individual subgraphs.
  - RDF: you need to differentiate between and manage multiple subgraphs in your dataset
    - has predefined schema vocabularies with well-understood data modelling semantics for specifying class and property schema elements
    - predefined domain-specific vocabularies such as vCard, FOAF, Dublin Core and SKOS for describing resources in different domains
    - designed to make it easy to share and publish data with fixed, well-understood semantics.
      - To qualify an edge in RDF with additional data, you must use intermediate nodes or blank nodes
    - supports the concept of named graphs, allowing you to group a set of RDF statements and identify them with a URI

#### Property Graph

- Apache tinkerpop's gremlin traversal language
- check the neptune property graph file

#### Resource Description Framework (RDF)

- w3c's SPARQL query language

### security

- network isolation via VPC
  - endpoints only accessible within VPC/peered connections
- security groups: firewall for the associated Neptune DB instance;

#### encryption

- data at rest in the database is encrypted using industry standard AES-256 KMS
  - Keys can also be used, which are managed through KMS

## considerations

- db engine version
- instances
  - instance class
  - burstable
  - notebook configuration: enables access to the cluster
- high availability: can be disabled
- db instance identifier: must be unique per account per region
- vpc, subnets, security group
- db cluster identifier, db port (e.g. 8192), parameter group
- iam authnz, encryption at rest: both can be disabled/enabled at creation
- failover
- backup retention period & window
- version maintenance and window
- sagemaker
  - direct access

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

### SageMaker

- for analyzing neptune graphs

### Glue

- getting data into neptune

### OpenSearch

- Amazon OpenSearch Service for full-text search capabilities

### DMS

- migrating data to a neptune cluster

### Backup

### EC2

- autoscaling groups for read replicas

### cloudwatch
