# developer associate

- last updated in Q4/2023 for version 1.0 DVA-C02
- bookmark:
  - Block Storage Knowledge Badge Assessment
    - ebs deep dive performance > Choosing the Best Amazon EC2 Instance for Your Workload
  - Exam Prep Official Practice Question Set
  - take the exam
    - [offical online practice exam](https://explore.skillbuilder.aws/learn/course/internal/view/elearning/14196/aws-certified-developer-associate-official-practice-exam-dva-c02-english)
    - [schedule the exam](https://aws.amazon.com/certification/certified-developer-associate/)

## links

- [bunch of links and study guides](https://aws.amazon.com/certification/certified-developer-associate/)
- [official exam guide pdf](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)
- [official sample questions pdf](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Sample-Questions.pdf)
- [officla practice question set](https://explore.skillbuilder.aws/learn/course/external/view/elearning/13757/aws-certified-developer-associate-official-question-set-dva-c02-english)
- [official ramp up guide (pdf)](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Developer.pdf)
- [worshop labs](https://catalog.us-east-1.prod.workshops.aws/)

## basics

- know the exam format
  - 65 questions
  - 130 minutes (2 minutes per question)
  - $150

### high level

> The exam validates a candidate’s ability to demonstrate proficiency in developing, testing, deploying, and debugging cloud-based applications.

- Develop and optimize applications on AWS
- Package and deploy by using continuous integration and continuous delivery (CI/CD) workflows
- Secure application code and data
- Identify and resolve application issues

### topics

- Domain 1: Development with Services 32%
- Domain 2: Security 26%
- Domain 3: Deployment 24%
- Domain 4: Troubleshooting and Optimization 18%

### Focus

> IMO: these are recurring themes

#### in general

> especially how these domains map to specific services

- analyzing & classifying application workloads
- architectural patterns
  - microservices, event driven, orchestration, sync vs async
  - REST, graphql, messaging, fanout
- distributed systems
- database architectures
- classifying, managing and protecting
  - data
  - runtime environments
- SAML, OpenID Connect, Active Directory
- Encryption, tokens, certificates
- analytics: observability, centralized & structured logging, code instrumentation & tracing, root cause analysis
- ci/cd: deployment strategies, artifact management
- devops: configuration as code, infrastructure as code

#### knowledgable

> what they do, main features, compare & contrast

- machine learning
  - macie, sagemaker, codeguru

#### in depth

> i mean deeper than deep with a focus on web & mobile application -> design, development, monitoring & observability, ci/cd, service integration, runtime performance

- all of these service domains:
  - authnz
  - well architected framework
  - compute
  - containers
  - serverless
  - dbs
  - storage
  - caching
  - APIs
  - security: encryption, certificates
  - ci/cd: automation, testing
  - development tools: apis, sdks, cli
  - analytics: observability, monitoring, visualization
  - application integration

#### technologies

> These servicies and technologies are recommended by AWS

- Analytics
  - Athena
  - Kinesis
  - OpenSearch Service
- Application integration
  - AppSync
  - EventBridge (CloudWatch Events)
  - Simple Notification Service (SNS)
  - Simple Queue Service (SQS)
  - Step Functions
- Compute
  - EC2
  - Elastic Beanstalk
  - Lambda
  - Serverless Application Model (SAM)
- Containers
  - Copilot
  - Elastic Container Registry (ECR)
  - Elastic Container Service (ECS)
  - Elastic Kubernetes Services (EKS)
- Cost and capacity management
- Database
  - Aurora
  - DynamoDB (NoSQL)
  - ElastiCache (NoSQL)
  - MemoryDB for Redis
  - RDS
- Developer tools
  - Amplify
  - Cloud9
  - CloudShell
  - CodeArtifact
  - CodeBuild
  - CodeCommit
  - CodeDeploy
  - CodeGuru
  - CodePipeline
  - CodeStar
  - X-Ray (instrumentation)
- Management and governance
  - AppConfig
  - Cloud Development Kit (CDK)
  - CloudFormation
  - CloudTrail (api monitor)
  - CloudWatch
  - CloudWatch Logs
  - Command Line Interface (CLI)
  - Systems Manager
- Networking and content delivery
  - API Gateway
  - CloudFront
  - Elastic Load Balancing
  - Route 53
  - VPC
- Security, identity, and compliance
  - Certificate Manager (ACM)
  - Certificate Manager Private Certificate Authority
  - Cognito
  - Identity and Access Management (IAM)
  - Key Management Service (KMS)
  - Secrets Manager
  - Security Token Service (STS)
  - WAF
- Storage
  - Elastic Block Store (EBS)
  - Elastic File System (EFS)
  - S3
  - S3 Glacier

## Domains

### 1: Development with Services

- architectural and fault-tolerent design patterns
  - stateful vs stateless concepts
  - tightly coupled vs loosely coupled components
  - sync vs async patterns
  - retries with exponential backoff and jitter, dead letter queues
  - event driven; microservices; coreography and orchestration; fanout

#### develop code for apps hosted on aws

- Architectural patterns (e.g., event-driven, microservices, monolithic, choreography, orchestration, fanout)
- Idempotency
- Differences between stateful and stateless concepts
- Differences between tightly coupled and loosely coupled components
- Fault-tolerant design patterns (e.g., retries with exponential backoff and jitter, dead-letter queues)
- Differences between synchronous and asynchronous patterns
- Creating fault-tolerant and resilient applications in a programming language (e.g.,
  Java, C#, Python, JavaScript, TypeScript, Go)
- Creating, extending, and maintaining APIs (e.g., response/request transformations, enforcing validation rules, overriding status codes)
- Writing and running unit tests in development environments (e.g., using AWS
  Serverless Application Model)
- Writing code to use messaging services
- Writing code that interacts with services by using APIs and SDKs
- Handling data streaming by using services

#### develop code for lambda

- Event source mapping
- Stateless applications
- Unit testing
- Event-driven architecture
- Scalability
- The access of private resources in VPCs from Lambda code
- Configuring Lambda functions by defining environment variables and parameters (e.g., memory, concurrency, timeout, runtime, handler, layers, extensions, triggers,
  destinations)
- Handling the event lifecycle and errors by using code (e.g., Lambda Destinations,
  dead-letter queues)
- Writing and running test code by using services and tools
- Integrating Lambda functions with services
- Tuning Lambda functions for optimal performance

#### using data stores in app development

- Relational and non-relational databases
- Create, read, update, and delete (CRUD) operations
- High-cardinality partition keys for balanced partition access
- Cloud storage options (e.g., file, object, databases)
- Database consistency models (e.g., strongly consistent, eventually consistent)
- Differences between query and scan operations
- DynamoDB keys and indexing
- Caching strategies (e.g., write-through, read-through, lazy loading, TTL)
- S3 tiers and lifecycle management
- Differences between ephemeral and persistent data storage patterns
- Serializing and deserializing data to provide persistence to a data store
- Using, managing, and maintaining data stores
- Managing data lifecycles
- Using data caching services

### 2: security

#### implement authnz for apps & services

- Identity federation (e.g., Security Assertion Markup Language [SAML], OpenID
  Connect [OIDC], Cognito)
- Bearer tokens (e.g., JSON Web Token [JWT], OAuth, Security Token Service)
- The comparison of user pools and identity pools in Cognito
- Resource-based policies, service policies, and principal policies
- Role-based access control (RBAC)
- Application authorization that uses ACLs
- The principle of least privilege
- Differences between managed policies and customer-managed policies
- Identity and access management (IAM)
- Using an identity provider to implement federated access (e.g., Cognito, Identity and Access Management [IAM])
- Securing applications by using bearer tokens
- Configuring programmatic access to AWS
- Making authenticated calls to services
- Assuming an IAM role
- Defining permissions for principals

#### implement incryption using services

- Encryption at rest and in transit
- Certificate management (e.g., Certificate Manager Private Certificate Authority)
- Key protection (e.g., key rotation)
- Differences between client-side encryption and server-side encryption
- Differences between managed and customer-managed Key Management Service (KMS) keys
- Using encryption keys to encrypt or decrypt data
- Generating certificates and SSH keys for development purposes
- Using encryption across account boundaries
- Enabling and disabling key rotation

#### manage sensitive data in application code

- Data classification (e.g., personally identifiable information [PII], protected health information [PHI])
- Environment variables
- Secrets management (e.g., Secrets Manager, Systems Manager Parameter
  Store)
- Secure credential handling
- Encrypting environment variables that contain sensitive data
- Using secret management services to secure sensitive data
- Sanitizing sensitive data

### 3: deployment

#### Prepare application artifacts to be deployed to AWS

- Ways to access application configuration data (e.g., AppConfig, Secrets Manager, Parameter Store)
- Lambda deployment packaging, layers, and configuration options
- Git-based version control tools (e.g., Git, CodeCommit)
- Container images
- Managing the dependencies of the code module (e.g., environment variables, configuration files, container images) within the package
- Organizing files and a directory structure for application deployment
- Using code repositories in deployment environments
- Applying application requirements for resources (e.g., memory, cores)

#### test applications in dev environments

- Features in services that perform application deployment
- Integration testing that uses mock endpoints
- Lambda versions and aliases
- Testing deployed code by using services and tools
- Performing mock integration for APIs and resolving integration dependencies
- Testing applications by using development endpoints (e.g., configuring stages in API Gateway)
- Deploying application stack updates to existing environments (e.g., deploying an AWS
  SAM template to a different staging environment)

#### automate deployment testing

- API Gateway stages
- Branches and actions in the continuous integration and continuous delivery (CI/CD) workflow
- Automated software testing (e.g., unit testing, mock testing)
- Creating application test events (e.g., JSON payloads for testing Lambda, API Gateway, SAM resources)
- Deploying API resources to various environments
- Creating application environments that use approved versions for integration testing (e.g., Lambda aliases, container image tags, Amplify branches, Copilot environments)
- Implementing and deploying infrastructure as code (IaC) templates (e.g., SAM templates, CloudFormation templates)
- Managing environments in individual services (e.g., differentiating between development, test, and production in API Gateway)

#### deploy code using ci/cd services

- Git-based version control tools (e.g., Git, CodeCommit)
- Manual and automated approvals in CodePipeline
- Access application configurations from AppConfig and Secrets Manager
- CI/CD workflows that use services
- Application deployment that uses services and tools (e.g., CloudFormation, Cloud Development Kit, SAM, CodeArtifact, Copilot, Amplify, Lambda)
- Lambda deployment packaging options
- API Gateway stages and custom domains
- Deployment strategies (e.g., canary, blue/green, rolling)
- Updating existing IaC templates (e.g., SAM templates, CloudFormation templates)
- Managing application environments by using services
- Deploying an application version by using deployment strategies
- Committing code to a repository to invoke build, test, and deployment actions
- Using orchestrated workflows to deploy code to different environments
- Performing application rollbacks by using existing deployment strategies
- Using labels and branches for version and release management
- Using existing runtime configurations to create dynamic deployments (e.g., using staging variables from API Gateway in Lambda functions)

### 4: toubleshooting and optimizing

#### root cause analysis

- Logging and monitoring systems
- Languages for log queries (e.g., CloudWatch Logs Insights)
- Data visualizations
- Code analysis tools
- Common HTTP error codes
- Common exceptions generated by SDKs
- Service maps in X-Ray
- Debugging code to identify defects
- Interpreting application metrics, logs, and traces
- Querying logs to find relevant data
- Implementing custom metrics (e.g., CloudWatch embedded metric format [EMF])
- Reviewing application health by using dashboards and insights
- Troubleshooting deployment failures by using service output logs

#### instrument code for observability

- Distributed tracing
- Differences between logging, monitoring, and observability
- Structured logging
- Application metrics (e.g., custom, embedded, built-in)
- Implementing an effective logging strategy to record application behavior and state
- Implementing code that emits custom metrics
- Adding annotations for tracing services
- Implementing notification alerts for specific actions (e.g., notifications about quota limits or deployment completions)
- Implementing tracing by using services and tools

#### optimize apps via services and features

- Caching
- Concurrency
- Messaging services (e.g., Simple Queue Service, Simple Notification Service)
- Profiling application performance
- Determining minimum memory and compute power for an application
- Using subscription filter policies to optimize messaging
- Caching content based on request headers
