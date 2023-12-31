# RDS

- distributed relational database supporting 7 different RDBMDS's like postgres, mysql, oracle, mariadb and sql server
- [aurora](./rds-aurora.md)
- [custom](./rds-custom.md)
- [proxy](./rds-proxy.md)

## my thoughts

## links

- [backup & restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html)
- [configuration](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_RDS_Configuring.html)
- [controlling access with security groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html)
- [dev resources](https://aws.amazon.com/rds/resources/)
- [high avialability multi az](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
- [instance types](https://aws.amazon.com/rds/instance-types/)
- [landing page](https://aws.amazon.com/rds/?did=ap_card&trk=ap_card)
- [security in RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)
- [security](https://docs.aws.amazon.com//AmazonRDS/latest/UserGuide/UsingWithRDS.html)
- [snapshots and restoring](https://docs.aws.amazon.com//AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html)
- [vpc: rds landing page](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html)
- [vpc: rds user guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)
- [working with read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)

## best practices

- read replicas vs multi-az vs scaling
  - read replicas: for scalability
    - provide both performance & availability benefits
    - you still need to implement a cache for appropriate requests
    - can be more cost effective than scaling out/up db instances
  - multi-az: for high availability
    - doesnt scale reads, since the standby cant be accessed directly
  - horizontal scaling
  - vertical scaling

### anti patterns

## features

- scale DB instance components: modify memory, processor size, allocated storage or IOPS individually individually
- enhanced availability and durability through the use of Multi-AZ deployments
- high availability, throughput, and storage scalability

### pricing

- by instance type
  - on demand: pay by hour
  - reserved: one or three year term
- by db engine
  - mysql
  - pg
  - mariadb
  - oracle
  - sqlserver
  - aurora: see the [markdown file](./rds-aurora.md)
- by storage options: Storage billed per gigabyte per month, and I/O is billed per million requests.
  - general purpose SSD
  - provisioned IOPS SSD
- by deployments
  - Outposts
  - Custom
  - Proxy
- data transfer costs is also determined by engine type: amount of data transferred to or from the internet and other AWS Regions

## terms

## basics

### db engine

- manages and runs all db operations

### db instance class

- determines the resources available to your instance
- the compute portion of RDS, the EC2 that runs the selected db engine
  - even tho the underlying service is EC2, you manage it via the RDS console
- instance class: how much memory, CPU, and I/O capabilities, in terms of network and storage throughput, will be available to the database engine
  - standard: m; balance of copmute, memory and network resources
  - memory optimized: r and x; for workloads that process large datasets in memory
  - burstable: t; ability to burst beyond the baseline performance

### storage

- the storage portion of RDS uses elastic block store for both data and log storage
- storage classes
  - general purpose ssd: gp2, gp3;
    - workloads running medium-sized db instances for dev and testing environments
  - provisioned iops ssd: io1;
    - i/o intensive workloads requiring low latency and consistent i/o throughput
  - magnetic: standard;
    - for backward compatibility; use one of the other classes

### high availability with Multi-AZ

- creates redundant instances of the databases in the same AZ different Availability Zones.
  - In the case of an infrastructure failure, performs an automatic failover to the standby in another Availability Zone
  - automates data replication & failover across AZs
- your apps connect to a single endpoint, and DNS handles routing to the secondary incase the primary fails
  - you need to ensure your app reconnects on failure with exponential backoff
    - if issues connecting to the secondary: update any cached DNS lookups

### snapshots

- automated backups are enabled by default
  - backups up the entire db instance and transaction logs
  - you need to set the backup window during a time the DB receives the least amount of activity
    - setting the retention period to 0 days disables automated snapshots and delete any existing backups
- manual bsnapshots: you can retain backups for longer than 35 days
- point-in-time recovery: create a new db instance using data restored from a specific snapshot

### security

#### Security Groups

- can use 3 types of security groups
  - VPC
  - EC2
  - database

#### Encryption

- at rest: can be enabled in configuration; uses the industry-standard AES-256 bit encryption
- in transit: enable SSL or TLS for securing communicatoin

## considerations

- create method
  - standard: set all available configuration options
  - easy: use recommended configuration options
- general workflow
  - pick db engine and configuration details (instance class, storage, master pass/name, etc)
  - create a VPC for the EC2 instance and the db engine
    - setup subnets
      - private: at least 2 private subnets in two distinct AZs
      - public: adds an EC2 instance in a third public subnet for connecting to the db in the private subnets
    - setup VPC security groups
  - IAM authnz
  - encryption
  - backup snapshots windows
  - monitoring
  - log exports to cloudwatch
  - version maintainence and time frames
  - deletion protection

## integrations

### lambda

- disaster recovery solution for RDS
  - create a scheduled AWS Lambda function to take database snapshots and store them in an S3 bucket.
  - configure Amazon SNS, as an event source for a second Lambda function, which copies the database into a second S3 bucket in a different Region.
- support real-time data analytics
  - creating a Lambda function that’s triggered each time an insert operation is run on the database
  - Lambda writes the data from the Amazon RDS record into an Amazon Kinesis Data Firehose stream
  - he stream of data is written to an S3 bucket

### vpc

- requires two subnets in two different Availability Zones for high availability.

### security groups

- restrict access at ec2, db, and VPC levels
- check the [markdown file](../networkingContentDelivery/securitygroups.md)

### IAM

- managing credentials: requires authNZ for access and table level permissions
- check the [markdown file](../securityIdentityCompliance/iam.md)

### ec2

### DMS

- database migration service
- migrate or replicate your existing databases to Amazon RDS
