# Global Architecture

- fundamental AWS architecture
- [tags](./tags.md)
- [local zones](./localZones.md)

## my thoughts

## links

- [global infrastructure intro](https://aws.amazon.com/about-aws/global-infrastructure/)
- [pricing calculator](https://calculator.aws/#/)
- [EU GDPR](https://gdpr.eu/what-is-gdpr/)
- [shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [aws repost: user community forum](https://repost.aws/)
- [top 10 security items](https://aws.amazon.com/blogs/security/top-10-security-items-to-improve-in-your-aws-account/)
- [billing and cost management](https://docs.aws.amazon.com/account-billing/index.html)
- [billing: user guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)
- [billing: consolidated](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html)
  [Title](https://docs.aws.amazon.com/wellarchitected/latest/framework/sustainability.html)
- [compliance center: research papers](https://aws.amazon.com/financial-services/security-compliance/compliance-center/?country-compliance-center-cards.sort-by=item.additionalFields.headline&country-compliance-center-cards.sort-order=asc&awsf.country-compliance-center-master-filter=*all)

### well architected

- [AAA landing page](https://aws.amazon.com/architecture/well-architected/)
- [AAA well architected PDF](https://docs.aws.amazon.com/pdfs/wellarchitected/latest/framework/wellarchitected-framework.pdf)
- [AAA: review process](https://docs.aws.amazon.com/wellarchitected/latest/framework/the-review-process.html)
- [FAQS](https://aws.amazon.com/well-architected-tool/faqs/)
- [lens: all of them](https://aws.amazon.com/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&wa-lens-whitepapers.sort-order=desc&wa-guidance-whitepapers.sort-by=item.additionalFields.sortDate&wa-guidance-whitepapers.sort-order=desc)
- [lens: custom](https://docs.aws.amazon.com/wellarchitected/latest/userguide/lenses-custom.html)
- [lens: financial services industry (FSI)](https://docs.aws.amazon.com/wellarchitected/latest/financial-services-industry-lens/welcome.html)
- [lens: intro](https://docs.aws.amazon.com/wellarchitected/latest/userguide/lenses.html)
- [lens: SaaS](https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/saas-lens.html)
- [lens: serverless](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)
- [network architecture](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/network-architecture-selection.html)
- [pillar: cost opitmization resources](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/cost-effective-resources.html)
- [pillar: cost optimization intro](https://wa.aws.amazon.com/wellarchitected/2020-07-02T19-33-23/wat.pillar.costOptimization.en.html)
- [pillar: cost optimization pricing models](https://docs.aws.amazon.com/whitepapers/latest/how-aws-pricing-works/aws-cost-optimization.html)
- [pillar: cost optimization right sizing](https://aws.amazon.com/aws-cost-management/aws-cost-optimization/right-sizing/)
- [pillar: cost optimization](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html?ref=wellarchitected-wp)
- [pillar: operational excellence](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/welcome.html?ref=wellarchitected-wpp)
- [pillar: performance efficiency](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/welcome.html?ref=wellarchitected-wp)
- [pillar: reliability whitepaper](https://wa.aws.amazon.com/wat.question.REL_6.en.html)
- [pillar: reliability](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html?ref=wellarchitected-wp)
- [pillar: security](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html?ref=wellarchitected-wp)
- [pillar: sustainability](https://docs.aws.amazon.com/wellarchitected/latest/framework/sustainability.html)

### tools

- [WAT: well architected tool](https://aws.amazon.com/well-architected-tool/)
- [WAT: user guide PDF](https://docs.aws.amazon.com/wellarchitected/latest/userguide/wellarchitected-ug.pdf)
- [cloud adoption framework](https://aws.amazon.com/cloud-adoption-framework/)

### guide

- [service endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#cfn_region)
- [service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)

## best practices

- resiliancy & availability
  - use region-scoped services in a minimum of two availability zones
- naming/tagging things
  - you generally always want to pre/postfix the resource type to the name of the resource, e.g. ec2-my-silver-server
  - you always want to have a tagging scheme predefined so you can query resources across service boundaries and for billing
- services and regions
  - you should specify a regional endpoint if the service supports it to reduce latency

### anti patterns

## basics

- Security: the practice of protecting your intellectual property from unauthorized access, use, or modification

### shared (security) responsibility model

- a good metaphor is an partment building
  - they are responsibile for the building, you are responsible for everything inside your apartment
- you: security **IN** the cloud:
  - securing your data, operating systems, networks, platforms, and other resources that you create
    - protecting the confidentiality, integrity, and availability of your data and for meeting any specific business or compliance requirements for your workloads.
  - requires you to monitor and manage the env at the operating system and higher layers
  - customer data
  - platform, applications, IAM
  - operating system, network and firewall configuration: e.g. patching and upgrades
  - customer-side data encryption and data integrity authnz
  - server-side encryption: file sytem/data
  - networking traffic protection: encryption, integrity identity
    - network and firewall configurations
- aws: security **OF** the cloud:
  - protecting the global infrastructure that runs all of the services offered in the AWS Cloud. This infrastructure comprises the hardware, software, networking, and facilities that run AWS services.
  - aws operates, manage and controls the components from the host operating system and virtualization layer down to the physical security of facilities inw hich services operate
  - software: up to the virtualization layer
    - compute, storage, database, networking
  - hardware / aws global infrastructure: the physical stuff, buildings, servers, private fiber cables, etc
    - regions, avaialbility zones, edge locations

### Console, CLIs and SDKs

- every action taken against an AWS resource is an API call
- SDKs are programming language specific
- in the console there is a new button to launch a cloud shell for using the cli

### global edge networks via cloudfront

- caching data closer to users to reduce latency
  - enables you to store data in a regions but cache them closer to users
- edge location: an endpoint for AWS that is used for caching content
- regional edge caching

### region & availbility zones

- region: geographical area that consists of two or more Availability Zones.
  - e.g. us-east-1
  - currently 3 region classifications: standard region,s china regions, and AWS GovCloud (US)
- availability zone: one or more interconnected data centers with redundant power, networking, connectivity, and so on.
  - e.g. us-east-1a and us-east-1b
- selecting a region
  - compliance: laws/company regulations requiring you to keep data in a specific geography
  - latency: relative to users and other services
  - pricing: AWS isnt for the little guy and varies from region to region
  - service availability: not all aws services are available in all regions

### Service Resiliency

- Globally-resilient services: operates globally with a single database
  - data is then replicated across AWS Regions
  - on failure, the service continues to operate because its replicated to other regions
- Regional-resilient services: operate in one Region with one set of that data in that Region
  - data is then replicated to multiple Availability Zones in that Region
  - if a single AZ fails, the other one picks up
  - if the region fails, your SOOL
- zone-resilient services: run in a single Availability Zone
  - if that AZ fails, your SOOL

## well architected framework

- helps you understand the pros and cons of decisions you make while building systems on AWS
- best practices for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud
- self-service tool that helps customers review AWS workloads at any time without the need for an AWS solutions architect.
- base Framework questions: foundational questions from the Framework that you review before applying any industry-specific questions in a lens review.

### Pillars

#### security

- the ability to protect data, systems, and assets while delivering business value through risk assessments and mitigation strategies
- dimensions: All AWS security services can be categorized by these five domains
  - identity and access management: ensure only authorized and authenticated users can access resources and do so as intended
  - detective controls: identify a potential security threat or incident
  - infrastructure protection: ensures systems and services within your workload are protected against unauthorized/unintended access and vulnerabilities
  - data protection: at rest and in transit via ecryption methods and access control and classifying data based on levels of sensitivity
  - incident response: processes in place to respond to and mitigate the impact of security incidents
- key goals
  - prevention: Define user permissions and identities, infrastructure protection, and data protection measures
  - detection: visibility into your organization’s security posture with logging and monitoring services
    - who has access
    - who executed
    - when and from where
    - evidence of
  - respond: Automate incident response and recovery to help shift the primary focus of security teams from responding to analyzing the root cause
  - remediate: event-driven automation to quickly remediate and secure your AWS environment in near-real time
    - ensure high availability
    - deploy apps with security and compliance-related configuration
    - apply security checks in a reproducible manner
- design principles
  - strong identity foundation built on the principles of least privilege
    - grant access only to those who need it
    - deny everything by default, and slowly open up based on roles, not specific users
  - enable tracability: monitor, alert and audit actions and changes in real time
  - apply security at all layers via defense in depth
    - network
    - application
    - data store
  - automate security best practices via APIs
    - identity management
    - network and data security
    - monitoring/observability capabilities
  - protect data in transit and at rest
    - creating and controlling the encryption keys used to encrypt your data
    - selecting appropriate encryption methods
    - validating integrity
  - minimize your attack surface
  - prepare for security events
    - processes to respond to and mitigate the potential impact of security incidents
    - tools and access in place ahead of a security incident
    - practice incident response through game days

##### CIA Triad

- Confidentiality: limiting information access and disclosure to authorized users (the right people) and preventing access by unauthorized people.
- Integrity: maintaining the consistency, accuracy, and trustworthiness of data over its entire life cycle
- Availability: the readiness of information resources. a system that is not available when you need it is almost as useless as not having a system in the first place

##### Principle of Least Privelege

- giving a user or system only those privileges that are essential to perform its intended function
- grant access as needed: start with denying access to everything and grant access as needed based on job role.
- enforce separation of duties: Set expectations on how authority will be delegated down through software engineers, operations staff, and other job functions involved in cloud adoption.
  - authorization for each interaction with resources
- avoid long-term credentials: use of temporary security credentials that eventually expire
  - centralizing privilege management
  - reducing or even eliminating reliance on long-term credentials

#### Operational Excellence

- running and monitoring systems to deliver business value and continually improving processes and procedures
- Define your metrics, set target goals, define and enforce your tagging strategy
- automating changes, responding to events, and defining standards to manage daily operations.

#### reliability

- ensuring that a workload performs its intended function correctly and consistently when it’s expected to
- resliency: ability to prevent and quickly recover from failures to meet demand
- 5 phases
  - generate: data through monitoring, gathering metrics and establishing thresholds
  - aggregate: creation of a complete view from multiple resources
  - alert: leveraging real-time processing and alerting capabilities
  - storage: data managent and retention policies; e.g. where and for how long logs are stored
  - analyze: analytics; dashboards, reports, and useful insights

#### performance efficiency

- using IT and compute resources efficiently

#### Cost Optimization

- measure and monitor your infrastructure and ensure cost-allocations are accurate
  - tagging is critical: use cost allocation tags
- data transfer charges are often overlooked
  - no charge for inbound data transfer across all services in all Regions
  - data transfer from AWS to the internet is charged per service, with rates specific to the originating Region.
- design principles
  - implement cloud financial management: dedicate necessary time to truly adopt cloud expenditure managemnet
  - adopt a consumption model: pay only for the computing resources you consume; avoid all other payment models
  - measure overall efficiency: measure the business output of the workload and the costs associated with delivery
  - stop spendig money on undifferentiated heavy lifting: focus on customers and projects rather than IT infrastructure
  - analyze and attribute expenditure: accurately identify the cost and usage of workloads

#### sustainability

- recommendations and strategies to use when designing cloud architectures that maximize efficiency and reduce waste

### Well architected tool

- a central place to perform and organize your framework reviews
- general process
  - define your workload
  - select the pillars to evaluate: each pillar provides a different set of questions
    - Consider each pillar and its importance to your business context.
  - select the base framework questions
    - at this point you can also select the an industry/tech specific lens
  - identify best practices you are following
    - at this point you can also identify lens-specific best practices as well
  - creates a summary report and an improvement plan with:
    - prioritized list of issues/recommendations
    - links to videos and documentation concerning best practices
    - generated report that summarizes your workload review
  - make improvements and measure progress
    - Create a project plan for adopting the improvements.

#### Lenses

- provide a way for you to consistently measure your architectures against best practices and identify areas for improvement
- A workload can have one or more lenses applied. Each lens has its own set of questions, best practices, notes, and improvement plan.
- pick a lens that matches your industry and workload type, or add a custom lens
  - each extends the guidance offered by the Framework to specific industry and technology domains

##### AWS Well-Architected Framework Lens

- automatically applied to all workloads

##### Serverless Lens

- focuses on designing, deploying, and architecting your serverless application workloads in the AWS Cloud
- covers scenarios such as RESTful microservices, mobile app backends, stream processing, and web applications

##### SaaS Lens

- focuses on designing, deploying, and architecting your software as a service (SaaS) workloads in the AWS Cloud

##### Custom Lens

- abcd

##### Financial Services Industry (FSI) Lens

- is based on design principles and best practices that AWS has developed from working with financial institutions around the world
- financial services industry has a unique set of risks and regulatory requirements
- design principles
  - documented operational planning
    - establish clear roles and responsibilities across management domains (operation, risk, compliance, auditors, etc)
  - automated infrastructure and application deployment
  - security by design
    - implement security baselines, configurations, and audit capabilities
  - automated governance
    - includes account provisioning, enforcing and monitoring budgets, and managing security, risk and compliance
- common workloads and best practices
  - accessing financial data: historical and real-time market data, alternative data, and buying decisions.
    - Strict requirements around user entitlements and data redistribution
    - low latency requirements
    - Reliable network connectivity for market data providers and exchanges
  - simplifying regulatory reporting: Building a reporting data lake on AWS and using the rich set of services
    - Data quality, integrity, and lineage into the ingest and processing pipelines
    - Data encryption at rest and in transit requirements
    - Mask or tokenize personally identifiable information (PII) data
  - using AI/ML: tools to enhance customer interactions through chatbots, improve surveillance, detect fraud, gather trading ideas from unstructured data, and customize product offerings.
    - Secure architecture to protect code and model artifacts
    - CI/CD pipeline integrated with change-control systems for model deployment
    - Automated end-to-end evidence capture of the entire model development lifecycle

##### Data Analytics Lens

- abcd

## Cloud Adoption Framework (CAF)

- TODO: review and summarize the link up top

### Security Perspective

- think about security from a data perspective: think about the data you are protecting, how it is stored, and who has access to it.
  - data is the most sensitive resource, so you would place it at the end of a chain to introduce two/more layers of defense between attackers and your data.
    - e.g. in a VPC having 2/more subnets for a multi-tier architecture
- key domains
  - AWS Identity and Access Management (IAM): Define, enforce, and audit user permissions across AWS services, actions, and resources.
  - Detective control: Improve your security posture, reduce the risk profile of your environment, and gain the visibility you need to spot issues before they impact your business.
  - Infrastructure security: Reduce the surface area of the infrastructure you manage and increase the privacy and control of your overall infrastructure on AWS.
  - Data protection: Implement appropriate safeguards that help protect data in transit and at rest by using natively integrated encrypted services.
  - Incident response: Define and launch a response to security incidents as a guide for security planning.
