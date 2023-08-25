# dynamodb

- fully managed key-value/document db providing multi-Region, multi-master deployments, durability, high availability and autoscaling, ACID-compliant transactions.
- support petabytes of data and tens of millions of read/write requests per second
- [dynamodb API](./dynamodb-api.md)

## my thoughts

- getting dynamodb right is all about the designing the data model
  - you'll end up with more tables than you would prefer, smaller items than you prefer, and longer partition keys than you prefer
  - the time spent planning will bear fruit at runtime
  - read the best practices articles, then read it again and recite it before bedtime
- dynamodb is often used as the holygrail of document stores, but DocumentDB may be more appropriate if migrating from mongodb

## links

- [AAA best practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
- [accelerator (DAX)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html)
- [autoscaling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html)
- [client and server side encryption](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/client-server-side.html)
- [consistency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)
- [core components](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
- [data model partitioning](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.Partitions.html)
- [data protection](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/data-protection.html)
- [direct kms materials provider](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/direct-kms-provider.html)
- [encryption at rest](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/EncryptionAtRest.html)
- [encryption client: how it works](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/how-it-works.html)
- [encryption client: intro](https://docs.aws.amazon.com/dynamodb-encryption-client/latest/devguide/what-is-ddb-encrypt.html)
- [error handling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ErrorHandling.html)
- [error handling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.Errors.html)
- [faqs](https://aws.amazon.com/dynamodb/faqs/?da=sec&sec=prep)
- [IAM: authnz](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/authentication-and-access-control.html)
- [lambda: dynamic content management](https://github.com/aws-samples/aws-lambda-manage-rds-connections)
- [landing page](https://aws.amazon.com/dynamodb/?did=ap_card&trk=ap_card)
- [limits](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html)
- [partition keys: decisions](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)
- [partition keys: design for uniform load](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-uniform-load.html)
- [partition keys: write sharding](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-sharding.html)
- [provisioned throughput: R/W capacity](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html)
- [R/W capacity](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html)
- [secondary indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html)
- [setting up dynamodb local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)
- [streams and lambda triggers](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.Lambda.html)
- [streams and lambdas (tut)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.Lambda.Tutorial.html)
- [streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html)
- [tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithTables.html)
- [working with large attributes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-use-s3-too.html)

### development

- [dynamodb local: get the docker image](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)

### blogs

- [adaptive capacity for uneven data access patterns](https://aws.amazon.com/blogs/database/how-amazon-dynamodb-adaptive-capacity-accommodates-uneven-data-access-patterns-or-why-what-you-know-about-dynamodb-might-be-outdated/)

## best practices

- determine your consistency requirement for each READ request; default is eventual
- monitoring and troubleshooting: watch your metrics
  - autoscaling takes time to get right, ensure your target utilization matches the spikes in your request patterns
  - always include aws error codes in your cloudwatch logs
  - enable cloudtrail so that control-plane operations (mgmt ops) are tracked for later analysis
  - cloudwatch is required to monitor table performance
    - set alarms for tracking when specified metrics fall outside acceptable ranges
- global tables
  - provide extremely low latency to global clients
- indexes
  - global secondary indexes
    - useful for quick lookups and temporary tables
  - local seoncary indexes
    - use them sparingly
    - choose projections carefully and only those atributes you request frequently
- table TTL: auto expire items
  - expire old items to keep your storage cost and RCU consumption low (and its free)
    - this is often more cost effective than paying for the WCU to delete an item
- Using streams
  - if implementing item TTL, you can watch the stream data and copy items into cold storage (e.g. s3)
- Using DAX: reduces operational and application complexity
  - decreases the amount of RCUs required for a table, and smooths out spikey/inbalanced read loads
  - reduces dynamodb's response time from single-digit millisecond to sub-millisecond
    - reduce response times of eventually-consistent read workloads
    - increase throughput for read-heavy/bursty workloads
- table, item and item attributes
  - updating a single attribute in an item requires rewriting the entire item
    - the recommended item size is under 4kb
  - design your data model so requests are evenly distributed across partitions
    - small item size may mean many tables to help control throughput costs
    - avoid large items, and large item attributes
  - its all about choosing the correct partition key, which controls how data is distributed across partitions
    - colocate hot data to a table thats equally distributed across partitions
      - cold data should be deleted via TTL or moved to s3
  - the total throughput provisioned for a table is dividied equally across partitions
    - thus if your only R/W to a single partition, the throughput allocated to remaining partitions are unused
    - can add a random number / calculated value to each partition key when writing data

### anti patterns

- using dynamodb, but not designing for dynamodb

## features

- JSON document / key-value data structures
- event driven programming
- fully managing sharding enables horizontal scaling
- Millisecond performance and automatic multi-Region replication
- bursts: an adaptive capacity to borrow read throughput from less active keys to more demanding keys
- global tables for fully managed multi-region & multi-master deployments
- auto scaling based on target read/write utilization
- DAX: the dynamodb accelerator: integrated cache with dynamodb compatible api
- capture changes with dynamodb streams and index to other data stores

### pricing

- read/write throughput depending on capacity modes
  - on demand: billed for each read and write
    - you dont need to specify expected throughput
    - best if you create new tables with unknown workloads or have unpredictable application traffic.
  - provisioned: billed for read and write capacity & use auto scaling to automatically adjust your table’s capacity within those limits
    - not for each specific read/write
    - best if you have predictable application traffic or run applications whose traffic is consistent or ramps up and down gradually.
- data storage
- optional features

## basics

- dynamodb replicates (copies) of your data in fault-independent zones within a region
  - replication usually occurs within 1 second
  - availability of four 9s

### table design

- tables: the core data structure
- items: records in a table
  - updating a single attribute in an item requires rewriting the entire item, so a balance must be met
    - a single item cannot be read greater than 3k RCU, or written at more than 1k WCU
    - eventually consistent reads are levied at half the cost of strongly consistent reads
- attributes: columns in an item
  - primary key: either partition key or partition + sort
    - partition key: input to an internal hash function whose output determines which node (physical partition) the item is will be stored on
      - if no sort key is provided, the partition key must be unique
    - sort key: if provided, required in all items and makes the primary key a composite key (partition + sort)
      - All items with the same partition key value are stored together in sorted order by sort key value
      - allows multiple items to be queried as a collection, which simplifies access patterns.
    - FYI:
      - string, number or binary
      - to use maps/lists as part of a primary key, you must expose a copy of the entry directly as an attribute
  - each item can be up to 400kb;
    - the larger the item the higher probability of `hot` activity
- partition key
  - should result in an even distribution of items and traffic across the hash space
    - pick a partition key that is strongly unique (high cardinality) across all items
  - should reduce the amount of `hot` keys
    - i.e. dont use US.STATE if 90% of your users are in California
    - read the adaptive capacity blog post
- rolling tables: where a new table is created for the current period, e.g. monthly/quarterly/etc
  - older tables will only see traffic when the application specifically requests them
  - enables setting a lower provisioned capacities as tables age and eventualy moved to cold storage/dropped
- attribute size
  - reading/writing large objects results in hot activity localized toa single partition
  - prefer to keep item size between 1 and 4kb
    - storing large json blobs in s3 and indexing their location in dynamodb
    - storing large json objects as binary attributes, and keeping the smaller metadata attributes separately
    - decompose json objects into smaller pieces that can be stored as separate items
      - this occurs at the application level using the `scatter-gather` technique
- one-to-many tables: e.g.
  - avoid large string/number sets as item attributes, and prefer storing each set element in a separate table as items
  - helps reduce throughput because you wont need to fetch the entire set every time you fetch the item
    - now your base table can contain a reference to the many table
- varied access patterns
  - generally each item in a table should have attributes that are read with similar access patterns
    - if each item has large attributes that dont meet this requirment, split those attributers into a new table
      - e.g. instead of one large USER table, have user.preferenes, and user.blah, and user.bloop
    - its all about controlling item throughput
- Global tables
  - tables can be spread across multiple regions
  - has a higher costs for an SLA of 5 nines
  - multi master, and conflicts are resolved by a last-write-wins mechanism
  - doesnt support cross-region strong consistency
  - generally require autoscaling or big enough write capacity to carry all global writes and accommodate the replicated traffic
  - routing mechanisms
    - geo-routing: send global clients to whichever global endpoint is closest
- table TTL: auto expire items
  - doesnt consume any WCU

#### data types

- key value model
  - string, number, boolean, binary (base64 encoded), null, and unordered sets of the aforementioned
- json model
  - unordered maps (i.e. object) and unordered lists (i.e. array) of any JSON data type
  - a single item can be a json document
  - or each item in a JSON can be attributes of a json document

### secondary indexes

- either local or global; enable queries on attributes other than the tables primary key
- in general
  - consumption of throughput is based on secondary index for scanning
  - sparse indexes: an attribute used as an secondary index but is only contained in a subset of base table items
    - thus sparsely indexing the base table and optimizes the throughput consumption
  - each index requires an additional write, thus incuring additional WCU costs

#### local secondary index (LSI)

- An index that has the same partition key as the table but a different sort key
- an LSI index must be local to a partition key;
  - e.g. base table (pkey = name, sortkey = id, attr = date)
  - e.g. LSI (pkey = name, sortkey = date, attr = id)
- requirements
  - max 5 per table, defined on table creation and cannot be deleted
  - have a max partition size of 10gb
  - must use the same partition key defined in the base table
  - cannot have its own provisioned throughput

#### global secondary index (GSI)

- temporary indexes that can use totally different partition & sort keys
  - recommended > LSI unless you need strong consistency
  - when your done with the GSI, delete it
  - model data access patterns that differ from the original table
    - i.e. take a base table, and define a completely different primary key over the same data
- logically its a replication of the base table with an entirely different primary key (partition and/or sort)
- GSI back pressure: when the GSI write throughput is too low and causes throttling to your base table during writes
  - Reads are independent to the base table;
    - can be used to isolate heavy reads to GSI (e.g. for scanning)
- requirements
  - up to 20 per table, created/deleted at any time
  - do not provide strong consistency like LSIs
  - are not subject to the size limitation of LSIs
  - can be created and deleted dynamically unlike LSIs
  - do not require unique primary keys
  - have their own provisioned throughput managed separately from the table
  - only supports eventual consistency
  - only return attributes that are projected into the index

### modes

- controls how you are charged for read/write throughput and how you manage capacity

#### ondemand

- tables automatically scale r/w throughput
- capacity settings: none, its managed for you
- scaling behavior: instantly handles up to double the previous traffic peak
- table WILL be throttled if:
  - hot partitions exist
  - The table exceeds double its previous peak traffic within 30 minutes
  - The traffic exceeds the per-partition maximum
- cost considerations: set amount for each read and write; able to evaluate cost per transaction
- use cases
  - default serverless workloads

#### provisioned

- capacity settings: set R/W capacity units
- scaling behavior: all provisioned capacity is available
- throttling behavior: requests throttled upon breaching provisioned capacity settings
  - causes error, however the aws sdk has builtin retries + backoff
- cost considerations: set rate for the amount of provisioned capacity
- use cases
  - predictable/consistent workloads

##### provisioned + autoscaling

- automatically scales the provisioned capacity ONLY when the consumed capacity is higher than target utilization for 2 consistent minutes
  - a scale-down event is initiated when 15 consecutive data points for consumed capacity in CloudWatch are lower than the target utilization
  - After Application Auto Scaling is initiated
    - UpdateTable is invoked: could takes minutes to update the provisioned capacity for a table/index
- capacity settings: define lower + upper capacity limits and target utilization percentage (20-90%)
- scaling behavior: auto scales to meet target utilizatoin
- throttling behavior: very short bursts may be throttled but only for a few minutes
- cost considerations: same as provisioned without autoscaling
-

### Provisioned Throughput

- request throughput: read and write capacity per second
  - read and writes are managed separately
  - must be specified when you create a table; AWS provisions resources to ensure your settings can be met
    - you set a min, max and a target utilization (in percent)
- autoscaling: adjusts provisioned throughput in response to actual traffic patterns
  - is enabled by default and can be configured separately for the table and GSI
  - the target utilization setting is what drives a smooth reaction in autoscaling to match your target request throughput
- Throttling: prevents your application from consuming too many capacity units.
  - fails with an HTTP 400 Bad Request error and a ProvisionedThroughputExceededException.

#### Read Capacity Units (RCU)

- for 1 item (4kb/less) per second
  - 1 strongly consistent reads
  - 2 eventually consistent reads (twice as fast!)
- Transactional read requests require TWO RCUs for each read
  - costs twice as much!

#### Write Capacity Units (WCU)

- WCU: write capacity unit; 1 WCU = 1 (1kb/less) write per second
- Transactional write requests require two WCUs

### streams

- captures data modification events in DynamoDB tables
  - must be enabled
  - compatible with the kinesis client library
- shards are created with the stream as the data grows
- all writes are recorded in the stream like a changelog
  - you can specify the level of detail recorded
  - kept for up to 24 hours
  - create: entire item
  - update: before and after of modified attributes
  - delete: entire item

### DAX: dynamodb accelerator

- api-compatible in memory cache for dynamodb tables via a separate endpoint
  - for even faster R/W throughput
- highly-available cluster accessbile only in a VPC
- you put the cache infront of your dynamodb tables and point your integrations (e.g. lambda) to it
- write-through cache: items and updates written to cache are eventually consistent on the next read
  - strongly consistent reads are not cached
- use cases: any realtime ready-heavy workloads
  - trading
  - real time bidding

### security

#### encryption

- at rest
  - All data is fully encrypted at rest using encryption keys stored in AWS KMS.
  - Choose AWS service keys or customer managed keys when you create a table.
- in transit: abcd

### backups

- backups: neither type consumes any read/write capacity
  - on demand: created when you request it
  - PITR: point in time recovery
    - 35-day rolling window of recoverable table data down to the second
- restore
  - always made to a new table, after which you can delete the previous one
  - most restores complete in less than 10 hrs
  - partitioned data is restored in parallel

### Troubleshooting

- console: view table and item settings, query tables directly,
- cloudwatch: monitoring
- cli: provides much more data than the console; useful for automating data collection
- cloudtrail: actions taken by a user, role, or an AWS service in DynamoDB

#### Throttling

- solutions:
  - avoid creating hot partitions by monitoring with Cloudwatch Contributor Insights
    - partition limits of 3000 RCU or 1000 WCU (or a combination of both) per second are exceeded.
  - add jitter and exponential backoff to your API calls
  - requests with several peak times and abrupt workload spikes should use ondemand NOT provisioned + autoscaling
- DynamoDB rate limits are applied per second
  - DynamoDB reports minute-level metrics to CloudWatch
  - thus 60 WCU === 3600 writes per minute, but you cant execute 3600 writes in one second
- R/WCU per minute might be lower than the provisioned throughput for the table
  - if all the workload falls within a couple of seconds then the requests might be throttled.
- Application Auto Scaling is not a suitable solution to address sudden spikes in traffic with DynamoDB tables.
  - It only initiates a scale-up when two consecutive data points for consumed capacity units exceed the configured target utilization value in a 1-minute span.

## considerations

- Tables
- Items
- Attributes
- Primary key
- Read/write capacity mode

## integrations

- with dynamodb streams you can easily integrate with SNS/SQS/Lambda etc for complex patterns

### IAM

- control access at the table and item levels.

### KMS

- end-to-end enterprise-grade encryption for data that is both in transit and at rest

### cloudwatch

### cloudtrail

- create a trail, you can set up continuous delivery of CloudTrail events to an S3 bucket, including events for DynamoDB.
- don't configure a trail, you can still view the most recent events in the CloudTrail console in Event history.
