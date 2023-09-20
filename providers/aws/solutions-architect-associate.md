# solutions architect associate

- last updated in Q4/2023 for version 1.0 SSA-C03
- bookmark:
  - Exam Prep: Module 4
  - Exam Prep Official Practice Question Set
  - Examp Prep Enhanced Course
  - Solutions Architect - Knowledge Badge Readiness Path
  - LAB: Deep dive: VPC peering > deep dive 2.pdf
  - new courses
    - Architecting on AWS: Online Course Supplement
    - AWS Technical Essentials
- todos
  - FYI: just by opening a lab marks it as completed, check the completed list and figure out which ones you still need to do

## links

- [landing page](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
- [exam guide PDF](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf)
- [exam: sample questions](https://explore.skillbuilder.aws/files/a/w/aws_prod1_docebosaas_com/1695236400/LWLUBaC6tfrPldPxaDhbug/item/e1e4bbc8acaee3f1ea7f6d62d2b52b4c33145088.pdf)

## basics

- exam is intended for individuals who perform in a solutions architect role
- know the exam format
  - 65 questions
  - 130 minutes (2 minutes per question)
  - $150

### high level

> The focus of this certification is on the design of cost and performance optimized solutions, demonstrating a strong understanding of the AWS Well-Architected Framework

- Design solutions that incorporate AWS services to meet current business requirements and future
  projected needs
- Design architectures that are secure, resilient, high-performing, and cost-optimized
- Review existing solutions and determine improvement

### Topics

- Domain 1: Design Secure Architectures 30%
- Domain 2: Design Resilient Architectures 26%
- Domain 3: Design High-Performing Architectures 24%
- Domain 4: Design Cost-Optimized Architectures 20%

### Focus

> IMO: these are recurring themes

- Well Architected Framework & Tool, Global Architecture, Shared Responsibility Model
- theres no fkn way to ignore EKS, nomad forever!
- public, private, hybrid and multi-cloud environments
- AWS Organizations & Accounts
- which aws services are Global, Regional and Zonal
- Providing and securing credentials
- STS, Roles, Federation, Active Directory
- IAM, Policy types, interpreting policy documents and all policy elements

#### technologies

> These servicies and technologies are recommended by AWS

##### Architectural Topics

- Compute
- Cost management
- Database
- Disaster recovery
- High performance
- Management and governance
- Microservices and component decoupling
- Migration and data transfer
- Networking, connectivity, and content delivery
- Resiliency
- Security
- Serverless and event-driven design principles
- Storage

##### AWS Services

- Analytics:
  - Athena
  - AWS Data Exchange
  - AWS Data Pipeline
  - EMR
  - AWS Glue
  - Kinesis
  - AWS Lake Formation
  - Managed Streaming for Apache Kafka (MSK)
  - OpenSearch Service (Elasticsearch Service)
  - QuickSight
  - Redshift
- Application Integration:
  - AppFlow
  - AWS AppSync
  - EventBridge (CloudWatch Events)
  - MQ
  - Simple Notification Service (SNS)
  - Simple Queue Service (SQS)
  - AWS Step Functions
- AWS Cost Management:
  - AWS Budgets
  - AWS Cost and Usage Report
  - AWS Cost Explorer
  - Savings Plans
- Compute:
  - AWS Batch
  - EC2
  - EC2 Auto Scaling
  - AWS Elastic Beanstalk
  - AWS Outposts
  - AWS Serverless Application Repository
  - VMware Cloud on AWS
  - AWS Wavelength
- Containers:
  - Elastic Container Registry (ECR)
  - Elastic Container Service (ECS)
  - ECS Anywhere
  - Elastic Kubernetes Service (EKS)
  - EKS Anywhere
  - EKS Distro
- Database:
  - Aurora
  - Aurora Serverless
  - DocumentDB (with MongoDB compatibility)
  - DynamoDB
  - ElastiCache
  - Keyspaces (for Apache Cassandra)
  - Neptune
  - Quantum Ledger Database (QLDB)
  - RDS
  - Redshift
  - Timestream
- Developer Tools:
  - AWS X-Ray
- Front-End Web and Mobile:
  - AWS Amplify
  - API Gateway
  - AWS Device Farm
  - Pinpoint
- Machine Learning:
  - Comprehend
  - Forecast
  - Fraud Detector
  - Kendra
  - Lex
  - Polly
  - Rekognition
  - SageMaker
  - Textract
  - Transcribe
  - Translate
- Management and Governance:
  - AWS Auto Scaling
  - AWS CloudFormation
  - AWS CloudTrail
  - CloudWatch
  - AWS Command Line Interface (AWS CLI)
  - AWS Compute Optimizer
  - AWS Config
  - AWS Control Tower
  - AWS License Manager
  - Managed Grafana
  - Managed Service for Prometheus
  - AWS Management Console
  - AWS Organizations
  - AWS Personal Health Dashboard
  - AWS Proton
  - AWS Service Catalog
  - AWS Systems Manager
  - AWS Trusted Advisor
  - AWS Well-Architected Tool
- Media Services:
  - Elastic Transcoder
  - Kinesis Video Streams
- Migration and Transfer:
  - AWS Application Discovery Service
  - AWS Application Migration Service (CloudEndure Migration)
  - AWS Database Migration Service (AWS DMS)
  - AWS DataSync
  - AWS Migration Hub
  - AWS Server Migration Service (AWS SMS)
  - AWS Snow Family
  - AWS Transfer Family
- Networking and Content Delivery:
  - CloudFront
  - AWS Direct Connect
  - Elastic Load Balancing (ELB)
  - AWS Global Accelerator
  - AWS PrivateLink
  - Route 53
  - AWS Transit Gateway
  - VPC
  - AWS VPN
- Security, Identity, and Compliance:
  - AWS Artifact
  - AWS Audit Manager
  - AWS Certificate Manager (ACM)
  - AWS CloudHSM
  - Cognito
  - Detective
  - AWS Directory Service
  - AWS Firewall Manager
  - GuardDuty
  - AWS Identity and Access Management (IAM)
  - Inspector
  - AWS Key Management Service (AWS KMS)
  - Macie
  - AWS Network Firewall
  - AWS Resource Access Manager (AWS RAM)
  - AWS Secrets Manager
  - AWS Security Hub
  - AWS Shield
  - AWS Single Sign-On
  - AWS WAF
- Serverless:
  - AWS AppSync
  - AWS Fargate
  - AWS Lambda
- Storage:
  - AWS Backup
  - Elastic Block Store (EBS)
  - Elastic File System (EFS)
  - FSx (for all types)
  - S3
  - S3 Glacier
  - AWS Storage Gateway

## Domains

### 1: Design Secure Architectures

#### Design secure access to AWS resources.

- Access controls and management across multiple accounts
- AWS federated access and identity services (e.g. IAM & SSO)
- AWS global infrastructure
- AWS security best practices
- The AWS shared responsibility model
- Applying AWS security best practices to IAM users and root users (e.g. multi-factor authentication)
- Designing a flexible authorization model that includes IAM users, groups, roles, and policies
- Designing a role-based access control strategy (e.g. AWS STS, role switching, cross-account access)
- Designing a security strategy for multiple AWS accounts (e.g. AWS Control Tower,
  service control policies (SCPs))
- Determining the appropriate use of resource policies for AWS services
- Determining when to federate a directory service with IAM roles

#### Design secure workloads and applications.

- Application configuration and credentials security
- AWS service endpoints
- Control ports, protocols, and network traffic on AWS
- Secure application access
- Security services with appropriate use cases (e.g. Cognito, Amazon
  GuardDuty, Macie)
- Threat vectors external to AWS (e.g. DDoS, SQL injection)
- Designing VPC architectures with security components (e.g. security groups, route
  tables, network ACLs, NAT gateways)
- Determining network segmentation strategies (e.g. using public subnets and private
  subnets)
- Integrating AWS services to secure applications (e.g. AWS Shield, AWS WAF, AWS
  SSO, AWS Secrets Manager)
- Securing external network connections to and from the AWS Cloud (e.g. VPN, AWS
  Direct Connect)

#### Determine appropriate data security controls.

- Data access and governance
- Data recovery
- Data retention and classification
- Encryption and appropriate key management
- Aligning AWS technologies to meet compliance requirements
- Encrypting data at rest (e.g. AWS Key Management Service)
- Encrypting data in transit (e.g. AWS Certificate Manager using TLS)
- Implementing access policies for encryption keys
- Implementing data backups and replications
- Implementing policies for data access, lifecycle, and protection
- Rotating encryption keys and renewing certificates

### 2: Design Resilient Architectures

#### Design scalable and loosely coupled architectures

- API creation and management (e.g. API Gateway, REST API)
- AWS managed services with appropriate use cases (e.g. AWS Transfer Family, Amazon
  Simple Queue Service, Secrets Manager)
- Caching strategies
- Design principles for microservices (e.g. stateless workloads compared with stateful
  workloads)
- Event-driven architectures
- Horizontal scaling and vertical scaling
- How to appropriately use edge accelerators (e.g. content delivery network)
- How to migrate applications into containers
- Load balancing concepts (e.g. Application Load Balancer)
- Multi-tier architectures
- Queuing and messaging concepts (e.g. publish/subscribe)
- Serverless technologies and patterns (e.g. AWS Fargate, AWS Lambda)
- Storage types with associated characteristics (e.g. object, file, block)
- The orchestration of containers (e.g. Elastic Container Service,
  Elastic Kubernetes Service)
- When to use read replicas
- Workflow orchestration (e.g. AWS Step Functions)
- Designing event-driven, microservice, and/or multi-tier architectures based on requirements
- Determining scaling strategies for components used in an architecture design
- Determining the AWS services required to achieve loose coupling based on requirements
- Determining when to use containers
- Determining when to use serverless technologies and patterns
- Recommending appropriate compute, storage, networking, and database technologies based
  on requirements
- Using purpose-built AWS services for workloads

#### Design highly available and/or fault-tolerant architectures

- AWS global infrastructure (e.g. Availability Zones, AWS Regions, Route 53)
- AWS managed services with appropriate use cases (e.g. Comprehend,
  Polly)
- Basic networking concepts (e.g. route tables)
- Disaster recovery (DR) strategies (e.g. backup and restore, pilot light, warm standby,
  active-active failover, recovery point objectives, recovery time objectives)
- Distributed design patterns
- Failover strategies
- Immutable infrastructure
- Load balancing concepts (e.g. Application Load Balancer)
- Proxy concepts (e.g. RDS Proxy)
- Service quotas and throttling (e.g. how to configure the service quotas for a workload
  in a standby environment)
- Storage options and characteristics (e.g. durability, replication)
- Workload visibility (e.g. AWS X-Ray)
- Determining automation strategies to ensure infrastructure integrity
- Determining the AWS services required to provide a highly available and/or fault-tolerant
  architecture across AWS Regions or Availability Zones
- Identifying metrics based on business requirements to deliver a highly available solution
- Implementing designs to mitigate single points of failure
- Implementing strategies to ensure the durability and availability of data (e.g. backups)
- Selecting an appropriate DR strategy to meet business requirements
- Using AWS services that improve the reliability of legacy applications and applications not built
  for the cloud (e.g. when application changes are not possible)
- Using purpose-built AWS services for workloads

### 3: Design High-Performing Architectures

#### Determine high-performing and/or scalable storage solutions

- Hybrid storage solutions to meet business requirements
- Storage services with appropriate use cases (e.g., S3, Elastic File
  System, Elastic Block Store)
- Storage types with associated characteristics (e.g., object, file, block)
- Determining storage services and configurations that meet performance demands
- Determining storage services that can scale to accommodate future needs

#### Design high-performing and elastic compute solutions

- AWS compute services with appropriate use cases (e.g., AWS Batch, EMR,
  Fargate)
- Distributed computing concepts supported by AWS global infrastructure and edge services
- Queuing and messaging concepts (e.g., publish/subscribe)
- Scalability capabilities with appropriate use cases (e.g., EC2 Auto Scaling,
  AWS Auto Scaling)
- Serverless technologies and patterns (e.g., Lambda, Fargate)
- The orchestration of containers (e.g., ECS, EKS)
- Decoupling workloads so that components can scale independently
- Identifying metrics and conditions to perform scaling actions
- Selecting the appropriate compute options and features (e.g., EC2 instance types) to
  meet business requirements
- Selecting the appropriate resource type and size (e.g., the amount of Lambda
  memory) to meet business requirements

#### Determine high-performing database solutions

- AWS global infrastructure (e.g., Availability Zones, AWS Regions)
- Caching strategies and services (e.g., ElastiCache)
- Data access patterns (e.g., read-intensive compared with write-intensive)
- Database capacity planning (e.g., capacity units, instance types, Provisioned IOPS)
- Database connections and proxies
- Database engines with appropriate use cases (e.g., heterogeneous migrations,
  homogeneous migrations)
- Database replication (e.g., read replicas)
- Database types and services (e.g., serverless, relational compared with non-relational,
  in-memory)
- Configuring read replicas to meet business requirements
- Designing database architectures
- Determining an appropriate database engine (e.g., MySQL compared with
  PostgreSQL)
- Determining an appropriate database type (e.g., Aurora, DynamoDB)
- Integrating caching to meet business requirements

#### Determine high-performing and/or scalable network architectures

- Edge networking services with appropriate use cases (e.g., CloudFront, AWS
  Global Accelerator)
- How to design network architecture (e.g., subnet tiers, routing, IP addressing)
- Load balancing concepts (e.g., Application Load Balancer)
- Network connection options (e.g., AWS VPN, Direct Connect, AWS PrivateLink)
- Creating a network topology for various architectures (e.g., global, hybrid, multi-tier)
- Determining network configurations that can scale to accommodate future needs
- Determining the appropriate placement of resources to meet business requirements
- Selecting the appropriate load balancing strategy

#### Determine high-performing data ingestion and transformation solutions.

- Data analytics and visualization services with appropriate use cases (e.g., Amazon
  Athena, AWS Lake Formation, QuickSight)
- Data ingestion patterns (e.g., frequency)
- Data transfer services with appropriate use cases (e.g., AWS DataSync, AWS Storage
  Gateway)
- Data transformation services with appropriate use cases (e.g., AWS Glue)
- Secure access to ingestion access points
- Sizes and speeds needed to meet business requirements
- Streaming data services with appropriate use cases (e.g., Kinesis)
- Building and securing data lakes
- Designing data streaming architectures
- Designing data transfer solutions
- Implementing visualization strategies
- Selecting appropriate compute options for data processing (e.g., EMR)
- Selecting appropriate configurations for ingestion
- Transforming data between formats (e.g., .csv to .parquet

### 4: Design Cost-Optimized Architectures

#### Design cost-optimized storage solutions

- Access options (e.g., an S3 bucket with Requester Pays object storage)
- AWS cost management service features (e.g., cost allocation tags, multi-account
  billing)
- AWS cost management tools with appropriate use cases (e.g., AWS Cost Explorer,
  AWS Budgets, AWS Cost and Usage Report)
- AWS storage services with appropriate use cases (e.g., FSx, EFS,
  S3, EBS)
- Backup strategies
- Block storage options (e.g., hard disk drive volume types, solid state drive
  volume types)
- Data lifecycles
- Hybrid storage options (e.g., DataSync, Transfer Family, Storage Gateway)
- Storage access patterns
- Storage tiering (e.g., cold tiering for object storage)
- Storage types with associated characteristics (e.g., object, file, block)
- Designing appropriate storage strategies (e.g., batch uploads to S3 compared
  with individual uploads)
- Determining the correct storage size for a workload
- Determining the lowest cost method of transferring data for a workload to AWS storage
- Determining when storage auto scaling is required
- Managing S3 object lifecycles
- Selecting the appropriate backup and/or archival solution
- Selecting the appropriate service for data migration to storage services
- Selecting the appropriate storage tier
- Selecting the correct data lifecycle for storage
- Selecting the most cost-effective storage service for a workload

#### Design cost-optimized compute solutions.

- WS cost management service features (e.g., cost allocation tags, multi-account
  billing)
- AWS cost management tools with appropriate use cases (e.g., Cost Explorer, AWS
  Budgets, AWS Cost and Usage Report)
- AWS global infrastructure (e.g., Availability Zones, AWS Regions)
- AWS purchasing options (e.g., Spot Instances, Reserved Instances, Savings Plans)
- Distributed compute strategies (e.g., edge processing)
- Hybrid compute options (e.g., AWS Outposts, AWS Snowball Edge)
- Instance types, families, and sizes (e.g., memory optimized, compute optimized,
  virtualization)
- Optimization of compute utilization (e.g., containers, serverless computing,
  microservices)
- Scaling strategies (e.g., auto scaling, hibernation)
- Determining an appropriate load balancing strategy (e.g., Application Load Balancer
  [Layer 7] compared with Network Load Balancer [Layer 4] compared with Gateway Load
  Balancer)
- Determining appropriate scaling methods and strategies for elastic workloads (e.g.,
  horizontal compared with vertical, EC2 hibernation)
- Determining cost-effective AWS compute services with appropriate use cases (e.g.,
  Lambda, EC2, Fargate)
- Determining the required availability for different classes of workloads (e.g.,
  production workloads, non-production workloads)
- Selecting the appropriate instance family for a workload
- Selecting the appropriate instance size for a workload

#### Design cost-optimized database solutions

- AWS cost management service features (e.g., cost allocation tags, multi-account
  billing)
- AWS cost management tools with appropriate use cases (e.g., Cost Explorer, AWS
  Budgets, AWS Cost and Usage Report)
- Caching strategies
- Data retention policies
- Database capacity planning (e.g., capacity units)
- Database connections and proxies
- Database engines with appropriate use cases (e.g., heterogeneous migrations,
  homogeneous migrations)
- Database replication (e.g., read replicas)
- Database types and services (e.g., relational compared with non-relational, Aurora,
  DynamoDB)
- Designing appropriate backup and retention policies (e.g., snapshot frequency)
- Determining an appropriate database engine (e.g., MySQL compared with
  PostgreSQL)
- Determining cost-effective AWS database services with appropriate use cases (e.g.,
  DynamoDB compared with RDS, serverless)
- Determining cost-effective AWS database types (e.g., time series format, columnar
  format)
- Migrating database schemas and data to different locations and/or different database engines

#### Design cost-optimized network architectures

- AWS cost management service features (e.g., cost allocation tags, multi-account
  billing)
- AWS cost management tools with appropriate use cases (e.g., Cost Explorer, AWS
  Budgets, AWS Cost and Usage Report)
- Load balancing concepts (e.g., Application Load Balancer)
- NAT gateways (e.g., NAT instance costs compared with NAT gateway costs)
- Network connectivity (e.g., private lines, dedicated lines, VPNs)
- Network routing, topology, and peering (e.g., AWS Transit Gateway, VPC peering)
- Network services with appropriate use cases (e.g., DNS)
- Configuring appropriate NAT gateway types for a network (e.g., a single shared NAT
  gateway compared with NAT gateways for each Availability Zone)
- Configuring appropriate network connections (e.g., Direct Connect compared with
  VPN compared with internet)
- Configuring appropriate network routes to minimize network transfer costs (e.g.,
  Region to Region, Availability Zone to Availability Zone, private to public, Global Accelerator,
  VPC endpoints)
- Determining strategic needs for content delivery networks (CDNs) and edge caching
- Reviewing existing workloads for network optimizations
- Selecting an appropriate throttling strategy
- Selecting the appropriate bandwidth allocation for a network device (e.g., a single
  VPN compared with multiple VPNs, Direct Connect speed)
