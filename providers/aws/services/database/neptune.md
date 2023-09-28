# neptune

- fully managed serverless graph database for highly connected, multi-layered datasets
- [tinker pop property graph](./neptune-propertyGraph-tinkerPop.md)
- [Neo4j's openCypher property graph](./neptune-propertyGrpah-openCypher.md)
- [w3c sparwl RDF graph](./neptune-rdfGraph-w3cSparql.md)
- bookmark
  - [gremlin standards compliance](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-gremlin-differences.html)
  - [getting started](https://docs.aws.amazon.com/neptune/latest/userguide/graph-get-started.html)

## my thoughts

## links

- [AAA: best practices](https://docs.aws.amazon.com/neptune/latest/userguide/best-practices.html)
- [AAA: reference architecture](https://github.com/aws-samples/aws-dbs-refarch-graph)
- [appsync: workshop example](https://github.com/aws-samples/aws-appsync-calorie-tracker-workshop/)
- [audit logs](https://docs.aws.amazon.com/neptune/latest/userguide/auditing.html)
- [bulk load tutorial](https://docs.aws.amazon.com/neptune/latest/userguide/bulk-load-tutorial-IAM.html)
- [bulk load user guide](https://docs.aws.amazon.com/neptune/latest/userguide/bulk-load.html)
- [db clusters](https://docs.aws.amazon.com/neptune/latest/userguide/feature-overview-db-clusters.html)
- [elb: examples with neptune gremlin client](https://aws.amazon.com/blogs/database/load-balance-graph-queries-using-the-amazon-neptune-gremlin-client/)
- [events](https://docs.aws.amazon.com/neptune/latest/userguide/events.html)
- [getting started: 7 videos 9 hrs](https://pages.awscloud.com/AWS-Learning-Path-Getting-Started-with-Amazon-Neptune_2020_LP_0009-DAT.html)
- [getting started](https://docs.aws.amazon.com/neptune/latest/userguide/get-started.html)
- [instance status checks](https://docs.aws.amazon.com/neptune/latest/userguide/access-graph-status.html)
- [lambda: examples](https://docs.aws.amazon.com/neptune/latest/userguide/lambda-functions.html)
- [landing page](https://aws.amazon.com/neptune/?did=ap_card&trk=ap_card)
- [running local](https://docs.aws.amazon.com/neptune/latest/userguide/graph-notebooks.html)
- [security](https://docs.aws.amazon.com/neptune/latest/userguide/security.html)
- [skillbuilder course](https://explore.skillbuilder.aws/learn/course/internal/view/elearning/14165/getting-started-with-amazon-neptune)
- [slideshare: graph data model and queries with neptune](https://www.slideshare.net/AmazonWebServices/work-backwards-to-your-graph-data-model-queries-with-amazon-neptune-dat330-aws-reinvent-2018)
- [slideshare: migrating to neptune](https://www.slideshare.net/AmazonWebServices/migrating-to-amazon-neptune-dat338-aws-reinvent-2018?qid=54cd934d-a746-48de-97a4-84321f6250f8)
- [slideshare: onramp to graph dbs & neptune](https://www.slideshare.net/AmazonWebServices/onramp-to-graph-databases-and-amazon-neptune-dat335-aws-reinvent-2018?qid=e677d773-0cb1-452d-95a3-b0be4d1dc7d9)
- [storage](https://docs.aws.amazon.com/neptune/latest/userguide/feature-overview-storage.html)
- [tagging resources](https://docs.aws.amazon.com/neptune/latest/userguide/tagging.html)
- [tools and utilities](https://github.com/awslabs/amazon-neptune-tools)
- [transactions](https://docs.aws.amazon.com/neptune/latest/userguide/transactions.html)
- [user guide](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html)
- [kinesis: data stream example](https://github.com/aws-samples/amazon-neptune-samples/tree/master/gremlin/stream-2-neptune)

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

### Errors

- ConcurrentModificationException: occur when multiple concurrent requests attempt to modify the same elements in the graph
- ReadOnlyViolationException: occur if the client attempts to write to a database instance that is no longer the primary.

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

- see the KMS file

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

- Neptune only allows connections from clients located in the same VPC as the Neptune cluster.
- you have to one of:
  - direct access to the DB: an A/NLB otherwise
  - api access: api gateway + lambda
    - optionally run lambda outside a VPC and connect to neptune via an ELB

### IAM

- user authn at creation
- roles can be assigned at anytime (e.g. to load data from s3)

### kinesis

#### Data Streams

- in high write throughput scenarios improve the reliability, performance and scalability by
  - sending logical writes to an kinsesis Data Stream
  - Lambda function polls the stream and issues batches of writes to the underlying Neptune database.
- A Kinesis Data Stream is provisioned to accept write requests from client applications, which act as record producers.
- Clients can use the Amazon Kinesis Data Streams API or Kinesis Agent to write individual records to the data stream.
- An AWS Lambda function processes records in the data stream.
  - Create a Lambda function and an event source mapping.
  - The event source mapping tells Lambda to send records from your data stream to the Lambda function, which uses a Gremlin or SPARQL client to submit write requests to the Neptune cluster endpoint.
  - The Lambda function issues batch writes to Neptune. Each batch is executed in the context of a single transaction.
  - To increase the speed at which the function processes records, add shards to the data stream.
- best practices
  - implement record aggregation in the client, and deaggregation in your Lambda functions
    - check the kinesis file for the record aggregation link
  - Consider pulling large batches from the stream by configuring the batch size property in the event source mapping, but writing smaller batches to Neptune one after another in a single Lambda invocation.
    - e.g. pulling 1000 records from the stream, and issuing 10 batch writes, each of 100 records, to Neptune during a single Lambda invocation.
    - allows you to tune batch write size according to factors such as the instance size of the Neptune database leader node and the complexity of your writes, while reusing a connection for the several batch writes you issue to Neptune during a single Lambda invocation
    - be careful as errors causes the entire batch to fail
  - control concurrency by adjusting the number of shards in your Kinesis Data Stream.
    - e.g. two shards will result in two concurrent Lambda invocations, one per shard.
    - set the number of shards to no more than 2x the number of vCPUs on the Neptune leader node.
  - Increasing the number of shards in order to increase concurrency and throughput will therefore increase costs.
    - alternatively at the expense of additional engineering effort, you can increase concurrency using the threading model particular to your Lambda runtime.
  - ensure that logical writes are wholly independent of one another such that they can be executed out of insert order, or direct dependent writes to the same shard using partition keys to group data by shard.

### VPC

- requires atleast two subnets in two different Availability Zones for high availability
  - dude stop spamming this everywhere, this is a default for high availability

### SageMaker

- for analyzing neptune graphs

### Glue

- getting data into neptune

### OpenSearch

- Amazon OpenSearch Service for full-text search capabilities

### DMS

- migrating data to a neptune cluster

### EC2

- autoscaling groups for read replicas
  - hmm this might be AWS autoscaling groups and not EC2 autoscaling groups

### ELB

- for both A/NLB
  - your Neptune cluster is run in at least two subnets in two Availability Zones, with each subnet in a different Availability Zone.
    - enables external clients to access the Neptune cluster endpoint, which always points to the primary instance in the cluster.
    - To enable access to the reader endpoint, which load balances connections across all the read replicas in the cluster, you will need to either create a second
      - target group and a listener configured with a different port
      - load balancer with a different DNS name.
  - The Neptune DB subnet group spans at least two subnets in two Availability Zones.
- ALB vs NLB
  - number of hops
    - NLB: one hop.
    - ALB: two hops between the client and the Neptune instance
  - service types
    - NLB: fully managed
    - ALB: third party proxy
- best practices
  - Restrict access to your cluster to a range of IP addresses using the security groups attached to the Neptune instances.
  - further restricting access by enabling IAM database authentication, requiring all HTTP requests be signed using AWS Signature Version 4, which requires changes to the client.
    - the client must sign the request using Neptune's DNS and include an HTTP Host header whose value is <neptune-cluster-dns:port>
      - i.e. the client must know the Neptune cluster's DNS and port in addition to the load balancer's DNS and port.

#### ALB

- Web connections from external clients terminate on an Application Load Balancer in a public subnet.
- The load balancer forwards requests to HAProxy running on an EC2 instance. This EC2 instance is registered in a target group belonging to the ALB.
- HAProxy is configured with the Neptune cluster endpoint DNS and port. Requests from the ALB are forwarded to the primary instance in the database cluster.
- FYI:
  - aws expicitly states HAProxy, but i'm sure Envoy would work just as well
- best practices
  - increase the availability of the publicly available endpoint by enabling multiple Availability Zones for the ALB, adding multiple HAProxy instances in each AZ, and load balancing across the HAProxy instances by including all instances in each ALB’s target group.
  - To enable access to the reader endpoint configure path-based routing either in the ALB, which will allow you to route to different HAProxy instances, or in HAProxy itself.
    - With path-based routing, the client would add a path suffix – such as /reader or /writer – to the request URI.
      - e.g. use http://<alb-dns>:80/gremlin/reader.
      - You will need to rewrite the path to remove this suffix in the ALB or HAProxy before passing the request to the appropriate Neptune endpoint.

#### NLB

- Web connections from external clients terminate on a Network Load Balancer in a public subnet.
- The load balancer forwards requests to the Neptune cluster endpoint (which then routes to the primary instance in the database cluster).
- The target IP addresses of the cluster endpoint are refreshed on a periodic basis by a Lambda function.
- This Lambda function is triggered by a CloudWatch event. When it fires, the function queries a DNS server for the IP addresses of the Neptune cluster endpoint. It registers new IP addresses with the load balancer’s target group, and deregisters any stale IP addresses
- best practices
  - you must use SSL termination and have your own SSL certificate on the proxy server. NLB, although a Layer 4 load balancer, supports TLS termination.
  - new cluster IPs are registered with the NLB as soon as they are identified. and stale IPs are removed (e.g. via health checks)
    - maintain a candidate list of IP addresses to be deregistered using a file stored in an S3 bucket.
      - theres a blog post link in ELB file

### API Gateway

- API Gateway exposes API operations that accept client requests and execute your backend Lambda functions.
- check the lambda section

### lambda

- execute gremlin/sparql queries, which can be exposed as endpoints via api gateway
- Neptune's VPC security group is configured to allow access from the AWS Lambda security group on the Neptune cluster's port.
- AWS Lambda is configured to access resources in your VPC.
  - allows Lambda to create elastic network interfaces (ENIs) that enable your function to connect securely to Neptune.
- The Lambda VPC configuration information includes at least 2 private subnets, allowing Lambda to run in high availability mode.
- The VPC security group that Lambda uses is permitted to access Neptune via an inbound rule on the Neptune VPC security group.
- Code running in your Lambda function uses a Gremlin or SPARQL client to submit queries to the Neptune cluster's cluster, reader and/or instance endpoints.
- best practices
  - to enable external internet access for your function, configure your Lambda security group to allow outbound connections and route outbound internet traffic via a NAT gateway attached to your VPC.
  - Lambda functions that are configured to run inside a VPC incurs an additional ENI start-up penalty
    - address resolution may be delayed when trying to connect to network resources.
  - Use a single connection and graph traversal source for the entire lifetime of the Lambda execution context.
    - If the Gremlin driver you’re using has a connection pool, configure it to use a single connection.
      - Hold the connection in a member variable so that it can be resued across invocations.
      - Concurrent client requests to the function will be handled by different function instances running in separate execution contexts – each with its own member variable connection.
  - Handle connection issues and retry connections in your function code.
    - unexpected network events can cause this connection to be terminated abruptly.
    - code your function to handle these connection issues and attempt a reconnection if necessary.
  - If your Lambda function modifies data in Neptune,
    - adopt a backoff-and-retry strategy to handle ConcurrentModificationException and ReadOnlyViolationException errors.

### AppSync

### kms

### s3

### cloudtrail

### SNS

### Backup

### cloudwatch
