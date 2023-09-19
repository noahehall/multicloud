# Common AWS Architecture

- AWS comprises over 150 services: happy hunting ;)~
- TODOs
  - some sections still need cleanup

## links

- [10 things serverless architects should know](https://aws.amazon.com/blogs/architecture/ten-things-serverless-architects-should-know/)
- [AAA: Reference Architecture Diagrams](https://aws.amazon.com/architecture/reference-architecture-diagrams)
- [AAA: Solutions and Implementations](https://aws.amazon.com/solutions/)
- [AAA: prescriptive guidance](https://aws.amazon.com/prescriptive-guidance/?apg-all-cards.sort-by=item.additionalFields.sortDate&apg-all-cards.sort-order=desc&awsf.apg-new-filter=*all&awsf.apg-content-type-filter=*all&awsf.apg-code-filter=*all&awsf.apg-category-filter=*all&awsf.apg-rtype-filter=*all&awsf.apg-isv-filter=*all&awsf.apg-product-filter=*all&awsf.apg-env-filter=*all)
- [aws samples repo](https://github.com/aws-samples)
- [aws vpc connectivity options](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/welcome.html)
- [blog: api gateway category](https://aws.amazon.com/blogs/compute/category/application-services/amazon-api-gateway-application-services/)
- [blog: apis at scale](https://aws.amazon.com/blogs/architecture/how-to-architect-apis-for-scale-and-security/)
- [builders library](https://aws.amazon.com/builders-library/?cards-body.sort-by=item.additionalFields.sortDate&cards-body.sort-order=desc&awsf.filter-content-category=*all&awsf.filter-content-type=*all&awsf.filter-content-level=*all)
- [cloud computing concepts](https://aws.amazon.com/what-is)
- [data lakes on AWS](https://aws.amazon.com/solutions/implementations/data-lake-solution/)
- [elasticache for redis vs memorydb for redis](https://cloudwellserved.com/amazon-elasticache-for-redis-vs-amazon-memorydb-for-redis/)
- [incident response plans](https://aws.amazon.com/blogs/publicsector/building-a-cloud-specific-incident-response-plan/)
- [localstack](https://github.com/localstack/localstack)
- [security, identity & compliance best practices](https://aws.amazon.com/architecture/security-identity-compliance/?cards-all.sort-by=item.additionalFields.sortDate&cards-all.sort-order=desc&awsf.content-type=*all&awsf.methodology=*all)
- [serverless apps Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)
- [serverless express (check examples dir)](https://github.com/vendia/serverless-express)
- [service search](https://aws.amazon.com/products/)
- [youtube: databases on aws](https://www.youtube.com/watch?v=WE8N5BU5MeI&t=9s)
- [backup and recovery approaches on AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/welcome.html)

### Whitepapers

- [blue/green dpeloyments on aws](https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf)
- [CICD on AWS](https://docs.aws.amazon.com/whitepapers/latest/cicd_for_5g_networks_on_aws/cicd-on-aws.html)
- [CICD: best practices on aws](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/welcome.html)
- [cost optimization tools](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-laying-the-foundation/reporting-cost-optimization-tools.html)
- [disaster recovery architecture pt1](https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-i-strategies-for-recovery-in-the-cloud/)
- [disaster recovery architecutre pt2](https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-ii-backup-and-restore-with-rapid-recovery/)
- [disaster recovery options](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html)
- [distributed data management](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/distributed-data-management.html)
- [high availability](https://docs.aws.amazon.com/whitepapers/latest/real-time-communication-on-aws/high-availability-and-scalability-on-aws.html)
- [high performance computing with s3 PDF](https://d1.awsstatic.com/whitepapers/architecture/AWS-HPC-Lens.pdf?did=wp_card&trk=wp_card)
- [hybrid networking](https://docs.aws.amazon.com/wellarchitected/latest/hybrid-networking-lens/hybrid-networking-lens.html)
- [implementing microservices on AWS pdf](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/microservices-on-aws.pdf)
- [incident response plan guide pdf](https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
- [microservices on aws pdf](https://docs.aws.amazon.com/pdfs/whitepapers/latest/microservices-on-aws/microservices-on-aws.pdf)
- [optimizing enterprise economics with serverless architectures](https://docs.aws.amazon.com/pdfs/whitepapers/latest/optimizing-enterprise-economics-with-serverless/optimizing-enterprise-economics-with-serverless.pdf)
- [overview of deployment options on aws](https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/aws-deployment-services.html)
- [running containerized microservices pdf](https://d1.awsstatic.com/whitepapers/DevOps/running-containerized-microservices-on-aws.pdf)
- [serverless multi-tier architectures (PDF)](https://d1.awsstatic.com/whitepapers/AWS_Serverless_Multi-Tier_Architectures.pdf)
- [service overview (PDF)](https://docs.aws.amazon.com/pdfs/whitepapers/latest/aws-overview/aws-overview.pdf)
- [whitepapers](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/compute-services.html)
- [cost optimization](https://aws.amazon.com/architecture/cost-optimization/?cards-all.sort-by=item.additionalFields.sortDate&cards-all.sort-order=desc&awsf.content-type=*all&awsf.methodology=*all)

### service categories

- [application integration](https://aws.amazon.com/products/application-integration/)
- [compliance programs](https://aws.amazon.com/compliance/programs/)
- [compliance](https://aws.amazon.com/compliance/)
- [compute](https://aws.amazon.com/products/compute/)
- [cost management](https://aws.amazon.com/aws-cost-management/)
- [data lakes and analytics](https://aws.amazon.com/big-data/datalakes-and-analytics/)
- [databases](https://aws.amazon.com/products/databases/)
- [frontend and mobile](https://aws.amazon.com/products/frontend-web-mobile/)
- [hpc: high performance computing](https://aws.amazon.com/hpc/)
- [hybrid cloud](https://aws.amazon.com/hybrid/)
- [iot](https://aws.amazon.com/iot/)
- [machine learning](https://aws.amazon.com/machine-learning/)
- [management and governance](https://aws.amazon.com/products/management-and-governance/)
- [migration & transfer](https://aws.amazon.com/products/migration-and-transfer/)
- [networking and content delivery](https://aws.amazon.com/products/networking/)
- [networking](https://aws.amazon.com/products/networking/)
- [premium support programs](https://aws.amazon.com/premiumsupport/technology-and-programs/)
- [securing enterprise grade serverless apps video](https://www.youtube.com/watch?v=tYhUWbDZ4-8)
- [security, identity and compliance](https://aws.amazon.com/products/security/)
- [serverless](https://aws.amazon.com/serverless/)
- [storage](https://aws.amazon.com/products/storage/)

### Quickstart Partner Solutions

- [landing page](https://aws.amazon.com/quickstart/)

### best practices

- code/repo organization
  - instead of focusing on organizing functions, focus on organizing services
  - perhaps a repo per service, with the service broken down into multiple fns and their resource dependencies
- prod vs developer cloud environments
  - ci/cd should be in place whichever route you take
  - prod should always be isolated
  - most flexible: separate accounts for each developer
    - requires technical maturity to handle the security and cost implications
  - least flexible: single shared account for all develoeprs
    - as long as everyone is using immutable and isolated stacks, you should be fine
- serverless
  - 12 factor app (say this twice)
  - dont assume local storage exists, but code for ephemeral storage & stateless services
  - instantiate expensive objects outside event handler
  - ensure you can test locally
  - end-to-end integration testing as early as possible in the dev cycle
  - profile your app for bottle necks
- serverless architecture
  - dont `architect` for serverless, but bring your best practices with you
    - if you abstract away your business logic to a single entrypoint
      - you should be able to repurpose it for deployment to a lambda OR a container
  - refrain from putting app/biz logic in the api GW layer
  - dont implement workflows in lambdas, use stepfunctions
  - dont implement long running processes in lambdas, use containers
- integration
  - understand your component timeouts, e.g. api gateway and lambda have different hard limits
- amazon has a database engine for every occasion, choose wisely

## Quick Start Partner Solutions

- automated reference deployments built by Amazon Web Services (AWS) solutions architects and AWS Partners
- review the link up top

## Serverless

- become familiar with
  - the multitude of cloudwatch events
  - optimizing lambda and integration with SNS, SQS and eventbridge
  - consuming streams (kinesis/dynamodb) effectively
  - orchestration (happy path + rollbacks) with step functions
- in AWS:
  - APIs: API Gateway, appsync
  - compute: lambda, ecs fargate
  - dbs: dynamodb, neptune, timestream, qldb, rds proxy, aurora, redshift, opensearch
  - messaging: sns, sqs, kinesis, eventbridge
  - networking: cloudfront, route53
  - dev tools: SAM cli, cdk, cloudformation
  - storage: s3, efs
  - orchestration: step functions
  - analytics: athena, cloudwatch, cloudtrail, xray
    - real time analysis: kinesis data streams/dynamodb streams
    - historical trends: batch, AWS glue + s3 > athena/redshift spectrum

## ci/cd/testing

- you should also invest time in setting up localstack
- in AWS
  - cloudformation, sam, codecommit, codebuild, codedeploy, codepipeline
  - SSM distributor

## Security

- security value chain
  - Identify resources: config, systems manager
  - protect your resourceS: shield, vpc, firewall manager, secreets manager, IAM, IoT device defender, key managment service, WAF
  - detect & monitor: macie, inspector, guard duty
  - automate responses: lambda
  - investigate incidents: cloudwatch, cloud trail
  - recover: ebs snapshots, glacier archives
- maintaining a proper security and compliance posture: security hub
  - collecting findings, aggregate and format into a structure format for processing
  - prioritizing: organize and categorize
  - visualization: across the entire stack

### securing infrastructure

- VPC: network and host level boundaries
  - isolates your compute resources
  - provides multiple layers of defense
  - offers a secure VPN connection AWS
- Inspector: Managing system security configuration and maintenance of running components
  - identify application security issues
  - enforce security best practices
  - generates assessment reports
- IAM/Resource Policies: enforcing service-level protection
  - service endpoint security configuration
  - principle of least privelege
- Shield: protection against DDoS attacks
- WAF: configure rules to protect against web based attacks
- Firewall Manager: configure WAF across AWS accounts

### encryption

- data at rest & in transit
- in AWS
  - key management service
  - CloudHSM
  - Certificate Manager
  - Secrets Manager

### AuthNZ

- in AWS
  - Cognito
  - directory service
  - IAM
    - Tags: enable ABAC
    - Roles: enable RBAC
  - Identity Center

### Incident Response

- in AWS
  - SSM OpsCenter Remdiation/task management

### Compliance

- check the compliance service category and programs links up top
- in AWS
  - Config
  - SSM Inventory, PatchManager, etc
  - Events + SNS + Lambda

### Disaster Recovery

- in AWS
  - Snapshots: most services offer this
  - DLM: Data Lifecycle Manager:
    - automates procedures to back up Amazon EBS volumes using lifecycle policies
    - automate the creation, retention, and deletion of EBS snapshots and Amazon EBS-backed AMIs.
  - Backup: centralizes compliance and policy control for managing EBS volume backups

## Networking

- in AWS
  - Foundational Services
    - VPC
    - Transit Gateway
    - PrivateLink
  - Edge Networking
    - CloudFront
    - Route53
    - Global Accelerator
  - Network Security
    - Shield
    - WAF
    - Network Firewall
    - Firewall Manager
  - Application Networking
    - App Mesh
    - Api Gateway
    - Cloud Map
  - Hybrid Connectivity
    - Direct Connect
    - Site-to-Site VPN
    - Client VPN
    - Cloud WAN

### OSI Model in AWS

- Layer 1: physical layer: Direct Connect
- Layer 2: the hypervisor; software that allows the hardware to be consumed
  - you dont have access to the physical hardware, but can use the AWS cli that is layered on top for some layer 2 access
- Layer 3: the management layer; aka software defined networking
  - where you create accounts and start building using the console, cli and APIs
- Layer 4: service layer; s3, lambda, RDS, EC2, VPC, security groups, IAM, etc

### topologies

#### point to point

- private data connection securely connecting two/more locations for private data services
- does not traverse the public internet

#### bus

- in early networks, a single physical cable was used to create connections between locations

#### tree

- share information across a network with many servers
- the network signals transmitted by root nodes hit all computers at the same time

#### hub and spoke

- the network hub is in the middle, and spokes are all the connected devices on the outside
- used in small and large networks
- in AWS
  - VPN connections
  - Direct Connect Gateways
  - VPC Peering
  - transit gateways

#### mesh

- a simultaneous fully connected and partially connected design
- adds fault-tolerance and redundancy for load balancing/WAN requirmeents
- in AWS
  - transit gateway
  - VPC peering connections

#### ring

- will usually have multiple rings for redundancy
- used in Metro Area Networks (MANs)

#### hybrid

- abcd

## Storage

- AWS absorbs and manages the extra capacity requirements but you still need to account for them
  - formatting
  - file system
  - hardware failure protection
  - data protection
  - operating system overhead
- allocated: billed for the capacity you reserve
- consumed: billed for the actual capacity used

### Core Storage Services

#### block storage

- dedicated, low-latency storage for each host; analogous to direct-attached storage (DAS) or a Storage Area Network (SAN)
- provisioned with each virtual server and offer the ultra low latency required for high-performance workloads.
- in AWS
  - EBS (peristent, networked)
    - snapshots: incremental, point-in-time copies of your data stored on your EBS volumes
  - ec2 instance storage (ephemeral, direct attached)
    - The available storage type is directly tied to the EC2 instance type.
  - FSx for NetApp ONTAP also offers block storage services over an iSCSI access protocol.

#### file storage

- access shared files and require a file system;often supported with a Network Attached Storage (NAS) server
- in AWS
  - EFS: cloud native file storage; multi-Availability Zone file storage service that uses NFS access protocol.
  - FSx: implement managed files storage using the commonly available file systems for on-premises solutions.
    - FSx for Lustre: high performance computing (HPC) and machine learning (ML) workload
    - FSx for Windows File Server: Microsoft applications and Windows workloads.
    - FSx for NetApp ONTAP: provide both NetApp block and file storage
    - FSx for OpenZFS:
  - EC2 + EBS: create high-performance block storage for network file system

#### object storage

- in AWS
  - S3: different storage classes or tiers to match your price, access, and availability requirements
  - s3 glacier specifically for archival data

### Network Attached Storage (NAS)

- connects to a shared storage device across the entire network
- usually kept in a data center and provides file-level access
- in AWS
  - EFS
  - FSx

### Storage Area Network (SAN)

- adds block-level access
- in AWS
  - Elastic Block Store

### Edge and hybrid cloud storage

- hybrid: connect your on-premises applications and systems to cloud storage
- Edge: compute and storage solutions; remote or disconnected locations
- in AWS
  - edge
    - Snow
  - hybrid
    - Outposts
    - Storage Gateway
      - File Gateway mode:
        - connect your Amazon S3 bucket using either the Network File System (NFS) or Server Message Block (SMB) protocol with local caching
        - deploy as a virtual appliance or purchase a hardware appliance version.
    - DirectConnect: a dedicated network connection from on premise into AWS
      - higher throughput and secure data transfer without passing through the internet
      - can be partitioned into multiple virtual interfaces.

#### Edge: Local compute and storage

- use compute resources and storage services even when disconnected from AWS
- provide a data transfer platform to copy your data in to and out from the AWS
- in AWS
  - Snowball Edge devices: edge computing, data migration, and edge storage device for data collection, ML and processing; Storage for envs with intermittent connectivity or in remote disconnected locations
    - Storage Optimized: highest storage capacity
    - Compute Optimized: more available vCPUs with a lower storage capacity.
  - Snowcone devices: smallest member of the Snow family purpose-built for use outside of a traditional data center.
  - Snowmobile service: exabyte-scale data transfer service used to move large amounts of data to AWS
    - transfer up to 100 PB per Snowmobile, a 45-foot long ruggedized shipping container, pulled by a semitrailer truck

#### Hybrid: onpremise cloud

- in AWS
  - AWS Outposts: offers the same AWS infrastructure, services, APIs, and tools to virtually any data center, colocation space, or on-premises facility
    - built on top of EBS and S3: compute, storage, database, and other services run locally on Outposts

#### Hybrid: onpremise gateway

- connects on-premises users and applications using a software appliance with cloud-based storage
- provides integration between an organization’s on-premises IT environment and the AWS storage infrastructure
- use cases
  - moving backups to the cloud
  - using onpremise file shares backed by cloud storage
  - low-latency access to data in AWS for on-premises applications; Local caching reduces network latency for both read and write activities.
- in AWS
  - S3 file gateway: store application data files and backup images as durable objects in Amazon S3
  - FSx file gateway: optimizes on-premises access to fully managed, highly reliable file shares in Amazon FSx for Windows File Server
  - Volume gateway: cloud-backed iSCSI block storage volumes to your on-premises applications
  - Tape gateway: replace physical tapes on premises with virtual tapes in AWS without changing existing backup workflows

### Data lakes

- in AWS
  - s3: data store because of its virtually unlimited scalability. You can nondisruptively increase storage from gigabytes to petabytes of content.
    - store all data types in their native formats
    - multi-tenant environment: many users can bring their own data analytics tools to a common set of data
  - ec2: servers for non AWS analytic tools to process data stored in s3
  - query, process and catalogue data:
    - athena: ad hoc data discovery and SQL querying
      - analyze data directly in Amazon S3, using standard SQL.
      - process unstructured, semi-structured, and structured data sets.
    - redshift spectrum: complex queries and scenarios where a large number of data lake users want to run concurrent BI and reporting workloads.
      - run Amazon Redshift SQL queries directly against data stored in an Amazon S3-based data lake.
      - can ETL data into redshift to create a data warehouse
    - FSx Lustre: can link to S3 buckets, allowing you to access and process data concurrently from a high-performance file system.
      - use S3 as a raw data repository as well as a repository for processed data
      - process massive data sets at up to hundreds of gigabytes per second of throughput, millions of IOPS, and sub-millisecond latencies.
    - rekognition: image & video processing
    - glue: fully managed ETL service that makes it simple and cost-effective to categorize your data
      - organize, cleanse, validate, and format data for storage in a data warehouse or data lake.
      - Glue Data Catalog is an index to the location, schema, and runtime metrics of your data.
    - lambda: triggers populate dynamodb tables
    - dynamodb: object names and other metadata describing objects in s3
    - elasticsearch: search for specific assets, related metadata, and data classifications.
    - quicksight: easy visualization

### Data transfer and migration

- copy or transfer on-premises data to/from Storage services in AWS.
- SDKs/CLIs/3rd party tools
  - rsync
  - aws cli

#### file transfer

- file transfers directly into and out of Amazon S3 or Amazon EFS
- in AWS
  - AWS Transfer: file transfers in/out of S3/EFS using SFTP, FTPS, FTP

#### data synchronization & online transfer

- move your data into and out of the AWS Cloud and Amazon S3 via online, internet based, connections.
- in AWS
  - DataSync: online data transfer service moving data between on-premises storage systems and AWS and between AWS services
    - can transfer hundreds of terabytes and millions of files at speeds up to 10 times faster than open-source tools
    - migrate active data sets or archives to AWS, transfer data to the cloud for timely analysis and processing, or replicate data to AWS for business continuity.
  - Transfer family of services: fully managed support for file transfers directly into and out of Amazon S3
    - migrate your file transfer workflows to AWS by integrating with existing authentication systems
    - providing DNS routing with Amazon Route 53 so nothing changes for your customers and partners, or their applications.
  - S3 Transfer Acceleration: fast, easy, and secure transfers of files over long distances
    - CloudFront globally distributed edge locations, routing data to Amazon S3 over an optimized network path.
    - best suited for scenarios in which you want to transfer data to a central location from all over the world or transfer significant amounts of data across continents regularly
  - Kinesis Data Firehouse: stream data into Amazon S3
    - captures and automatically loads streaming data in Amazon S3 and Amazon Redshift,
  - Kinesis Data Streams: build custom applications that process or analyze streaming data for specialized needs
    - continuously capture and store terabytes of data per hour from hundreds of thousands of sources, such as website clickstreams, financial transactions, social media feeds, IT logs, and location -tracking events.
    - emit data from Kinesis Data Streams to other AWS services, such as Amazon S3, Amazon Redshift, Amazon EMR, and AWS Lambda.
  - Amazon partners: use third-party connectors additional Amazon transfer service support.

#### offline data transfer & migration

- physical storage method to move your data from remote locations to AWS
- run operations in austere, non-datacenter environments, and in locations where there’s lack of consistent network connectivity.
- in AWS
  - AWS Snow: offers several physical devices and capacity points, most with built-in computing capabilities
    - SnowCone: smallest member of the AWS Snow Family of edge computing, edge storage, and data transfer devices
      - run compute applications at the edge, and ship the device with data to AWS for offline data transfer
      - or you can transfer data online with AWS DataSync from edge locations.
    - SnowBall: edge computing, data migration, and edge storage device that comes
      - Edge Storage Optimized: block storage and Amazon S3-compatible object storage, along with 40 vCPUs
      - Edge Compute optimized: 52 vCPUs, block and object storage, and an optional GPU
    - Snowmobile: Exabyte-scale data transfer service used to move extremely large amounts of data to AWS
      - its a big fucking truck

#### data migration

- in AWS
  - Application Migration Service: MGN; migrating applications to the AWS Cloud, AWS GovCloud (US), and AWS Outposts.
    - fka CloudEndure

### Data protection

- services to meet your data redundancy and disaster requirement needs
- in AWS
  - Backup: centralized, end-to-end policy-based solution whenever you require auditing, reporting, and management,
  - service native snapshots: offered by the service, e.g. RDS has snapshot features
    - work best when you have no auditing requirements for the service and you are only required to back up a limited number of AWS services.
  - Elastic Disaster Recovery Service (DRS): specializes in disaster recovery planning

#### Backup & Archive

- in AWS
  - AWS Backup: centralize and automate data protection across AWS services

#### Snapshots

- built in to most services, check the docs for the storage service you're using
- generally always end up in a protected part of Amazon S3 as part of the managed service

#### Replication

- built into most services, check the docs for the storage service you're using

#### Disaster Recovery Services

- AWS DRS

## Compute

- configuration of
  - OS
  - CPUs
  - RAM
  - GPU
  - storage
  - networking

### VM instances

- EC2: run anything
  - the foundational building block of all AWS compute services
- use cases
  - granular and complete control of infrastructure
  - long running applications, long running computation cycles
  - compute-intensive or memory-intensive applications
  - stateful apps

### Containers

- EKS: k8s in the cloud
- ECS EC2: immutable apps
- use cases
  - fully managed with performance configurable infrastructure
  - compute-intensive workloads
  - microservices
  - auto scaling

### serverless

- lambda: serverless max 15 minute computation cycles
- ECS Fargate: serverless immutable services
- use cases
  - fully managed infrastructure with tunable infrastructure performance
  - focus on the code
  - event driven applications

## Analytics

### Monitoring and troubleshooting

- in AWS
  - cloudwatch: core supporting service within AWS which provides metrics, logs, and event management services
    - used throughout AWS services for health and performance monitoring and log management
    - also automatically troubleshoot and remediate incidents in real time.
  - CloudTrail: logs all API actions in your account to an S3 bucket
    - Management Events: control plane events are logged by default
    - Data Events: data plane events require additional setup

#### Network Monitoring

- in AWS:
  - Only CloudWatch and VPC Flow Logs TOETHER can collect logs to ensure you are meeting your compliance requirements and use the logs to identify any suspicious ports and IP addresses.
  - VPC flow logs: lets you to capture information about the IP traffic going to and from network interfaces in your VPC.
  - Traffic Mirroring: copy network traffic from an elastic network interface of an Amazon EC2 instance
    - can send the traffic to out-of-band security and monitoring appliances for further analysis
  - VPC Reachability Analyzer: network diagnostics tool that troubleshoots reachability between two endpoints in an Amazon VPC, or within multiple Amazon VPCs.
  - AWS Transit Gateway Network Manager: lets you to centrally manage your networks that are built around transit gateways.

### Tags

- tagging resources is foundational to all analytical services

## Databases

### Db migration

- in AWS
  - DMS: Database Migration Service
    - from non/aws db engines to aws dbs
  - SCT: Schema Conversion Tool
    - this is part of DMS, but focuses specifically on conversion schema and code across heterogeneous db engines

## Devops

### IaC

- in AWS: manage all your AWS resources programmatically.
  - cloudformation: terraform for aws
  - CDK: sdk for various programming languages to interact with the AWS control plane API
  - opsworks: create, configure, and manage your database servers.

### Self Service

- in AWS
  - Service Catalogue
  - SSM Automation

## Cloud Financial Management (CFM)

- everything stems from cost allocation tags and organizations
  - once you
    - allocate costs back to specific services
    - use organizations and accounts effectively to segment your activities
    - enforce a consistent policy and strategy across organizations and tag implementations
  - you can utilize other tools to automate the process of reporting
- in AWS
  - expenses and usage:
    - AWS Cost Explorer
    - Cost and Usage reports (must be enabled?)
    - the AWS Billing and Management Console
    - AWS Budgets
    - AWS Trusted Advisor
    - Compute Optimizer
  - governance
    - organizations
    - Control Tower

## Migration

- for migrating TO aws
- in AWS
  - Analysis
    - Migration Evaluator: migration assessment for lift and shift
      - after you life n shift, you can modernize
    - Migration Hub
    - Application Discovery Service: realtime analysis of live systems, app depending mapping, etc
  - Migration
    - Application Migration Service (fka cloudEndure)
