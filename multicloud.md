# multicloud

- theres a copypasta section at the bottom to cleanup

## links

- [api as a product](https://api-as-a-product.com/articles/case-study-human-centered-api-design/)
- [api product ideation and validation](https://medium.com/api-product-management/api-product-ideation-and-validation-aef140db00b)
- [api versioning (stripe)](https://stripe.com/docs/api/pagination/auto)
- [api versioning](https://stackoverflow.com/questions/389169/best-practices-for-api-versioning)
- [aws architecture center](https://aws.amazon.com/architecture/)
- [AWS leadership principles](https://www.amazon.jobs/en/principles?ref=wellarchitected-wp)
- [azure architecture center](https://docs.microsoft.com/en-us/azure/architecture/)
- [best practices for designing pragmatic RESTful apis (enchant)](https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
- [best practices for REST api design (stackoverflow.blog)](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/)
- [best practices in API Design (swagger)](https://swagger.io/resources/articles/best-practices-in-api-design/)
- [circuit breaker pattern](https://www.martinfowler.com/bliki/CircuitBreaker.html)
- [CRUD](https://en.m.wikipedia.org/wiki/Create,_read,_update_and_delete)
- [fan out/in integration pattern](https://dzone.com/articles/understanding-the-fan-out-fan-in-api-integration-p)
- [GCP patterns for scalablee and resilient apps](https://cloud.google.com/architecture/scalable-and-resilient-apps)
- [gcp: what is multicloud](https://cloud.google.com/learn/what-is-multicloud)
- [github architectural decision records](https://adr.github.io/)
- [grpc](https://grpc.io/)
- [HATEOAS driven REST APIs](https://restfulapi.net/hateoas/)
- [how to mke effective service blueprints](https://miro.com/guides/service-blueprints/)
- [http decision diagram](https://github.com/for-GET/http-decision-diagram)
- [human-centered api design](https://medium.com/api-product-management/design-apis-human-centered-to-build-successful-api-products-ffe35015cee5)
- [IBM: the GOAT of documentation](https://www.ibm.com/docs/en)
- [learning domain driven design](https://dddcommunity.org/learning-ddd/)
- [list of software architecture styles and patterns](https://en.wikipedia.org/wiki/List_of_software_architecture_styles_and_patterns)
- [microservice architecture patterns and best practices](http://microservices.io/index.html)
- [Network Address Translation](https://en.wikipedia.org/wiki/Network_address_translation)
- [network protocols](https://en.wikipedia.org/wiki/Lists_of_network_protocols)
- [network topoligies](https://en.wikipedia.org/wiki/Network_topology)
- [protobuf](https://protobuf.dev/)
- [rapid prototyping](https://engineeringproductdesign.com/knowledge-base/rapid-prototyping-techniques/)
- [REST (roy fielding)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
- [REST (wikipedia)](https://en.m.wikipedia.org/wiki/Representational_state_transfer)
- [REST API: concepts, best practices and benefits (altexsoft)](https://www.altexsoft.com/blog/rest-api-design/)
- [rest](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
- [restful api best practices](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
- [RESTful web API design (microsoft)](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [rethinking service blueprints for agile delivery](https://wiprodigital.com/2018/08/30/rethinking-service-blueprints-for-agile-delivery/)
- [roy fielding](https://roy.gbiv.com/)
- [selecting a rapid prototyping process](https://engineeringproductdesign.com/rapid-prototyping-process-selection-key-factors/)
- [Selina Liu: microservices @ airbnb](https://www.youtube.com/watch?v=PIw1WF1UXNc)
- [serverless framework](https://github.com/serverless/serverless)
- [shared nothing architecture](https://en.wikipedia.org/wiki/Shared-nothing_architecture)
- [state of devops 2021 (PDF)](https://services.google.com/fh/files/misc/state-of-devops-2021.pdf)
- [TCO: serverless vs traditional clouds](https://pages.awscloud.com/NAMER-field-GC-Deloitte-TCO-whitepaper-2019-learn.html)
- [the action pattern](https://ponyfoo.com/articles/action-pattern-clean-obvious-testable-code)
- [TOGAF capability framework](https://pubs.opengroup.org/architecture/togaf9-doc/arch/?ref=wellarchitected-wp)
- [towards a human-centered workshop design](https://www.slideshare.net/TobiasBlum/innovating-the-api-economy-towards-a-humancentered-workshop-design)
- [zachman framework](https://www.zachman.com/about-the-zachman-framework?ref=wellarchitected-wp)
- master thesis by tobias blum; creator of the human-centered api methdology

### ISO standards

- [popular ISO standards](https://www.iso.org/popular-standards.html)
- [iso 31000 risk management framework](https://www.iso.org/iso-31000-risk-management.html/)

## Infrastructure Design

- the document, diagram, or vision that encompasses all of the characteristics of your infrastructure that supports your hosted applications
- constraints and assumptions that address the `insert here` of systems and resources that support and host your business automation workflows
  - requirements
  - availability
  - manageability
  - performance
  - recoverability
  - security: data security patterns and a clear mapping of these patterns to Cloud security controls

### Hybrid cloud

- existing application that must run on premises, that uses databases or files, or that must perform backups, and you want to use cloud resources and scalability.
- fast, local access to data, but you also want to take advantage of cloud compute and analytics engines.
- years’ worth of security and compliance requirements, supported by processes and procedures in disparate systems on premises. And you want to use cloud management and monitoring capabilities, ideally from a single pane of glass.
- many physical locations to manage with data and applications and you want reliable connectivity and simplified maintenance.

### multiloud, cloudnative and open source

- opensource: often conflicts with responsibilities of proprietary cloudnative services
  - configuring VMS
  - updating operating systems
  - installing application runtime
  - build and deploy pipelines
  - scaling and load balancing
  - monitor and observe
  - data storage
- multicloud
  - requires attention to the network latency introduced for request pipelines that traverse cloud networking boundaries
  - pay attention to vendor lock-in if utilizing proprietary services

### server-based vs serverless architectures

- cost aside, its all about
  - where you want to spend your time
  - level of server/host/application customization/control required to fulfill business needs
  - technical maturity of team and access to subject matter experts

#### server based

- you manage all aspects of scalability, availablility, fault-tolerance, softare maintenance and host configuration
- use cases/considerations
  - predictable, consistent workloads with long-running/intensive computations
  - custom host configurations or software implementations
  - ability to manage, configure and maintain the underlying software

#### serverless

- abstracting away the compute infrastructure to the point you have no responsibilties for servers on which your code runs
- key characteristics: any cloud providers fully managed service can be part of a serverless architecture
  - infrastructure externally managed
  - high availability
  - horizontaly scales
  - redundant and fault tolerant
- use cases/considerations
  - purpose driven architectures and application flexibility
  - quick to market
- serverless architecture: thinking in terms of patterns and applications, rather than in terms of individual functions or resources
  - migration strategies
  - types of compute and data stores you can select
  - application architecture patterns you can use
- application design: choose services and patterns that suit your workloads based on characteristics such as
  - expected throughput
  - service limits
  - cost
  - SLO, SLAs
- cost comparisons with other architecture models
  - infrastructure cost to run workloads: e.g. server provisioning vs invocation costs
  - upfront development cost: dev effort to plan, architect and provision resources
  - maintainence cost:
- value comparisons with other architecture models
  - increased speed and agility: of course this comes after the initial learning curve
  - cost allocation: much easier to allocate costs to customers and events vs to servers
    - i.e. the costs occur when events occur
- think through how to observe and react to events in your distributed services
  - each component should scale independently
  - implement circuit breakers to restrict the blast radius of failed services

### cloud migration

- two broad domains: how do you
  - implement compute infrastructure:
    - capacity processes and cost models: reflects the three general ways of operating infrastructure
      - server based
      - containerized
      - APIs and microservices
  - approach application dev and deployment: operational processes and development models
    - simple move to the cloud: but lacks flexible build and deploy processes
    - api driven microservice-based applications with the most flexibility but requires the most rewrite of legacy tech stac
- considerations: its all about documentation, planning and strategy
  - what does each application do and how are the components organized
  - how can you break up data based on CQRS? you have to strangle the database along with the microservices
    - what belongs to the control plane?
    - what belongs to the data plane?
    - once the data is distributed, which set of db engines match the throughput, consistency, access patterns, etc reqirements
  - how does the application scale, and which components drive the capacity you need?
    - should you migrate leaf components first? or those with the most demanding capacity requirements
  - do you have scheduled based tasks?
  - do you have workers listening to a queue?
  - where can you refactor/enhance functionality without impacting the current implementing
    - perfect for load balancers and API gateway

#### migration patterns

- refactor/migration: service by service is moved to the cloud into a new architecture
- lift and shift: aka rehost; copy pasta legacy into cloud services to save on hosting costs
  - the idea is you take the entire legacy set of applications and move it into the cloud
  - the core benefit is reducing infrastructure costs (no more on premise) while taking advantage of cloud compute reliability & availability
- replatform: lift and shift then replacement/refactor
  - this is more incremental then a pure lift and shift
  - you will need to connect the remaining legacy services with the new cloud services until everything is fully migrated
- leap frog: from legacy on-premise monoliths straight to serverless cloud architecture
- organic: migration with a lift-and-shift approach
  - e.g. servers to EC2s, perhaps some ECS/lambdas thrown in, but limited rewrites
  - the goal is to get things into the cloud, and experiment with serverless & microservices
- strangler: incrementally and systematically decomposes monolithic applications by creating APIs and building event-driven components that gradually replace components of the legacy application.
  - Distinct API endpoints point to old and to new components and safe deployment options (such as canary deployments) let you point back to the legacy version with very little risk.

### deploying

- strategy for deploying new application versions and infrastructure are equally important
- QA, qa, Qa, qA ;)~
- standardization and optimation
- auditing and reacting to changes
- halt/rollback mechanisms
- managing configuration changes
  - similar in terraform how you can consume deployed artifacts (e.g. ARN, ips, etc)
    - bake it into the deployment package: refrain from this as much as possible
    - 12factor it: the easiest, but should atleast be encrypted; however its difficult to share/update across nodes
    - load at runtime from some secrets manager: the most robust (and complex) option

#### strategies

- all at once: 100% of traffic goes to the new version
- traffic shifting strategies
  - canary deployment: e.g. 20% goes to new versio for 20 mins, and then once validated 100% goes to new version
  - linear deployment: e.g. 10% of traffic goes to new version every 20 minutes until 100% is shifted

### testing

- testing: you generally need a test account that mirrors the production account
- local tests within the dev environment
  - unit tests focusing on business logic
  - cloud native code is generally more difficult to test locally (see localstack)
- integration tests: targeting remote test accounts with prod parity
- automated integration and accepted tests against other envornments providing gates for production deployments

## Uptime

- To design for availability and durability
  - understand the requirements of your workloads
  - the level of service that you've promised to your business operation teams.
- business continuity: the ability of an organization to provide useful services in the event of failure

### SLAs

- service level agreements that outline acceptable downtime or promised uptime
  - always includes audit and compliance objectives if stipulated by industry regulation/laws
- RTO: recovery time objective; maximum allowed time to restore a system to operational status
  - i.e. if RTO is 6 hrs, engineers have up to 6 hrs to restore a failed system
- RPO: recovery point objective; the maximum allowed time since the last data recovery point (backup)
  - i.e. if you backup every 10 hours, the greatest amount of data you can lose is the last 10 hours
- tier 1 SLA: Mission critical services: Outages have a severe impact on business operations.
  - usually require four to 5 nines or better of availability.
  - Usually include customer facing sites, revenue, or order processing systems
- Tier 2 SLA: Business critical services: Outages have a significant impact on business during operational hours.
  - Usually three nines of availability
  - Short outages may affect revenue but are not catastrophic
  - Any application that isn't driving revenue but that supports Tier 1 applications
- Tier 3 SLA: Business-operational, non-critical services: Workloads support internal business operations but are not customer facing.
  - Two nines of availability
  - Low impact workloads such as analytics
- Tier 4 SLA: Administrative non-critical services: Office productivity. There is no impact to customers if these workloads are down.
  - Uptime of 90 percent or better
  - Internal applications

### availability

- the percentage of time a system is accessible.
- availability is expressed as a percentage of uptime in a given year or as a number of nines
  - one nine: 90% uptime, 36.53 downtime
  - two nines: 99% uptime, 3.65 days downtime
  - three nines: 99.9% uptime, 8.77 hours downtime
  - 3 1/2 nines: 99.95% uptime, 4.38 hours downtime
  - 4 nines: 99.99% uptime, 52.60 minutes downtime
  - 4 1/2 nines: 99.995% uptime, 26.30 minutes downtime
  - 5 nines: 99.999% uptime, 5.26 minutes downtime
- availbility types
  - active-passive: one of two resources are considered the primary, the other secondary and becomes primary if the current one fails
    - challenges:
      - statefulness: acceptable since theres only one primary
      - availability: if failover fails, your resources wil be unreachable
      - scalability: since only one resource is considered primary, it will bear the brunt of the load
        - since the passive resource doesnt share the load; you need vertical scaling to accomodate increased demand
        - stop the passive resource > resize and restart > make primary and shift traffic> stop, resize and restart the new secondary
          - you repeat this process when demand decreases
  - active-active: more than one of many resources are considered primary
    - challenges
      - statefulness: managing state across resources can be difficult and should generally be stateless
        - push state into a load balancer that fronts the fleet of resources
      - scalability: since multiple resources share load you need _horizontal scaling_
        - automation is key; adding and removing resources should match demand in near real-time
- strategies
  - horizontal scaling: in/out; increasing the total number of load balanced resources
  - vertical scaling: up/down; increasing perf characteristics of existing resources

#### scaling

- strategize for the current scale requirements in addition to the anticipated growth
  - know the capabilities and service limits of the services that you’re integrating
    - especially the service limits, e.g. api gateay 10mb vs SQS 256kb
  - select patterns that optimize your application for the scale you need to support
    - timeouts, retry behavior, throughput, payload size, etc
- elasticity: horizontal scaling + automation to match demand

##### demand

- both compute + data
- organic demand
- merger and acquisition: increases dramatically within a short period

##### complexity

- management
- performance
- security

##### reliably with tests

- increased need for more effective load tests since you can scale/tweak the perf of individual components
  - you need to have a solid understanding of peak load capacity
- best practices
  - load test with authentic data with appropriate volumes that exercise each integration point with production access patterns
  - spin a production like environment, never load test locally
  - identify the bottlenecks/failure points, modify perf characteristcs and iterate
    - this may move the bottleneck to other integration points
      - you need to know where the bottleneck is most appropriate, based on your workload and business requirements
  - choose a percentile that reflects the business need when monitoring
    - build in error handling logic to handle failures that are outliers
  - dont mock services you cant control
    - when you're load testing, you need the real beef

##### managing scale through monitoring

- components should output appropriate signals
- monitor by percentile, and not just avg/raw numbers
- log efficiently and effectively

### Durability

- systems that can perform its responsibilities even in the presence of failures
- the capability of a system to handle failures (hardware, networking, regional power loss) while protecting data from deletion or loss.
- durable storage: stores data reliably without data loss even during component failures

### redundancy

- strategy for increasing durability and availabliity
- its all about duplicating data & servers across infrastructure in isolated geographic locations
- challenges
  - replication process: keeping data, configuration, etc in sync across primary and secondary resources
  - failover: redirecting traffic from primary to secondary on failure
    - DNS strategy: updating an IP addr to point to a different DNS record
      - be careful of DNS caching and the time it takes to propagate DNS changes
    - Load balancing strategy: the IP points to a load balancer that routes requests to health checked resources

### Fault Tolerance

- designing for 0 downtime
  - unlike with high Availability, where some downtime is acceptable
- systems that continue to operate even when dependent services are faulty

### Disaster Recovery

- systems that operate through and recovery from disaster
- spectrum of strategies ranging from backup & Restore to multi-site active/active
- Backups are an essential component of your business resiliency and operational commitments.
- developing a backup strategy for services and data
  - is there documentation on the data, applications and workloads in your environment
  - What data is required to keep the business running?
  - What additional data does the business want backed up?
  - What purpose does this backup data serve?
    - Is this for disaster recovery planning?
    - Compliance and audit purposes?
    - Daily business operations or data recovery?
    - Ransomware recovery?
  - How do you intend to use this backed-up data?
    - Allow users to recover their own files?
    - For development and testing?
    - Long-term retention? what are the retention requirements for each workload?
    - Regulatory compliance?
    - just in case backups?
- Backup granularity: defined by the depth the backup or restore requires.
  - File-level recovery: e.g. any configuration files needed to restore an application
  - Application-level recovery: e.g. a specific application version
  - Application data-level recovery: e.g. a specific database within MySQL
  - volume-level recovery: e.g. any persistent volumes used across nodes
  - instance-level recovery: e.g. any VMs or containers
  - managed-service recovery: e.g. if using some serverless/managed service

#### Data Protection

- designed to restore/preserve a copy of data from a desired point in time
- goals
  - Restore data in a timeframe meeting Recovery Objectives
  - recovery: protect it in case of a disaster, accident, or malicious action.
  - compliance: theres bunches of regulations depending on the industry and type of data stored
- Recovery Point Objectives: RPO; recovering to a specific point in time, usually measured in milliseconds
- Recovery Time Objectives: RTO; time it takes to recover, usually measured in minutes
- backups: creating efficient and cost-effective data copies
  - incremental
  - frequency of backups
  - capacity limits
  - expiration
- disaster recovery drills

#### Backup and Restore

- Using a pure backup and restore strategy is associated with longer recovery time and recovery point objectives
- results in longer downtime and greater loss of data between when the disaster event occurs and the recovery of the systems.
- the easiest and least expensive strategy to implement.
- use cases
  - tier 3 and 4 SLAs

#### Pilot Light

#### Warm Standby

#### Multi-site active/active

## virtualization

- container: a standard unit of software; the highest level of abstraction
  - the container runtime shares the hardware OS kernel
  - you create isolation via file system layers
  - provides the best utilization of the underlying hardware
    - you can share OS libraries and software, or isolate them
- hypervisor: soft/firmware that enables sharing physical hardware resources across one/more virtual machines
- virtual machine: emulates a physical server; virtualize the operating system and better utilize resources
  - each application can now be isolated from other applications at the VM level
  - the virtualization layer is still heavy as it requires a complete operating system
    - which is potentially redundant as isolation requires redundant copies of OS, and OS libraries
- baremetal: the most inefficient; hardware costs are the same regardless of resource utilization
  - applications fight for the same set of resources
  - libraries are generally shared across applications and require synchronization
- from baremetal, vms to containers in increasing levels of abstractions
  - baremetal
    - server hardware
  - vms
    - operating system
  - containers
    - containerization platform
    - libraries
    - applications
- syscall: inspect api calls made at the namespace level
- OOM killer: out of memory killer: algorithm for finding the oldest & largest process in the system, and kills it to freeup memory for the other processes
  - its difficult to find out which process has been killed by the OOM killer algorithm
  - stopping a process doesnt freeup the memory allocated to it, you have to kill it
- cgroup: control groups; deals with CPU and Memory
- orchestration: service that coordinates container deployment, placement strategy, failure, and resource utilization across mutiple hosts
  - scheduling and placement of containers
  - automatic scaling in/out based on some strategy
  - self-healing services by autoamtically removing unhealthy containers and deploying new ones
  - integration with cloud and other services, e.g. networking and persistence
  - security, monitoring and logging for the fleet

### best practices

- dont install openssh/etc in a container
  - instead log into a parent namespace and start a shell
  - openssh: bypasses PEM modules, user-management, cert & key management is a whole-nother issue, etc
- forking (executing) too many small processes that run then die can eat up cpu time becasue the scheduler cant keep up with the start/stop
- PIN CPUs to a namespace to provide a clear boundary at the container level
  - this is difficult to get right, google it
- stay away from page swapping for performance reasons
  - instead focus on proper memory management
- always thinka bout the container architecture
  - user-space: where your app lives: full container isolation
  - kernel: shared space: all containers in the system share these resources; be aware of noisy neighbors
    - dont believe all the continer isolation stuff, its still possible for a container to break out of this boundary into kernel land
    - all isolation occurs at the kernel level, protect the king
  - platform: this is the physical hardware
- consider which containers share a network namespace
  - sharing network namespace increases perf (no need to go through all the tcp handshakes and things)
  - ^ but also reduces your security posture (you dont go through all the tcp handshakes and things)
- managing users from the user-namespace is possible, but difficult to get right
- in general
  - small containers, and always start from scratch
    - starting from scratch forces you to choose your syslibraries wisely to match the needs of your application
  - nologin from the outside world (no openssh, always exec sh for access)
  - north-side communication must be guarded
  - container technology !== network encryption: always implement e2e encryption at the application level

### container security

- risk process frameworks
  - ISO 31000: risk management
- risk mitigation: considering which containers are on the same platform (hardware) & use the same namespaces
- risk assessment: isolation, segmentation and management of apps in containers, the containers themselves, and the system (kernel) and platform (hardware)
  - confidentiality: its all about segregation of communication
    - container to container
    - process to process
    - container to outside
  - access
    - Who/When/Where
    - Logging
    - Start/Stop
    - Content of the container image
  - integrity
    - kernel 2: cgroups & namespaces
    - kernel 3 and 4: namespaces v2 (you should focus here)
  - availability
    - Resource Usage: cpu, memory, data compression
      - memory management is handled globally; you need to ensure a container doenst consume all of the systems memory
    - Noisy Neighbor effect: in multitenant systems with shared resources, the activity of one tenant can negatively impact another tenant's share of resources
- namespaces: you need to use the clone systemcall to create a namespace for a container
  - types
    - PID-namespace: isolated process namespaces per PID
      - each process has a global and local PID
      - the root namespace can see a child processes global and local pid
      - inside a container only the processes local PID is visible
      - the first process in a namespace has local id PID0, and its incremented by 1 and linked
        - killing a process kills its process tree
    - CPU/Memory-namespace (cgroup): its all about memory management and CPU scheduler
      - policy-based scheduling: kernel system scheduler has a policy for executing tasks on a specific cpu
      - CPU Affinity: pins a task to a specific CPU, instead of using policy-based scheduling
      - memory limitation: Out-of-memory OOM killer; do you kill processes that exceed their specified memory limit?
      - dirty/used/empty pages
      - context: memory is attached directly to a specific CPU, switching/migration takes time
        - context switching: the cpu scheduler moves from one cpu to another
        - context migration: the cpu scheduler decides a process needs to move from one cpu to another cpu
          - the memory used by a container IS NOT migrated with the process, so this impacts performance
      - cpu threads
    - Network-namespace: can be shared across PID-namespaces for communication across processes
      - puts network intefaces into namespaces: processes in a namespace can only use the available interfaces for connectivity
        - all processes in the network-namspace can talk to the inteface
      - kernel responsibilities: occurs at the kernel level, and thus global to all containers in the system
        - routing/fowarding/filters/bridging still happens in the kernel
        - TCP/UDP/ICMP stacks
    - User-namespace: allows containers to have their own user-ids, which are mapped to global userids
      - mapping table for users for local and global context
    - FS/Mount-namespace: the filesystem namespace
      - mapping table for filesystem paths
        - from the container perspective: it looks like a local path
        - from the kernel perspective: its an overlay to a system path
          - use syscall for open to inspect it
    - IPC-namespace: system file/inter-process communication; two containers shouldnt use IPC for comms, theres always a better way
    - UTS-namespace: allows a container to have its own hostname
  - organization: a tree structure; processes of different namespace-tree-branches cant see other branches
    - root > child x..y > a child can create a child
      - but a child does not have visibility into a cousin, e.g. cousin X cant kill cousin Y

## event driven architectures

- uses events to communicate between and evoke decoupled services
  - state and code are decoupled
  - integration through a messaging layer enabling asynchronous connections
- events: an observable (change in state) that contains all the information required to take subsequent action
- event producers: entities that create and publish events, e.g. websites, apps, etc to unknown consumers usually through an event-bus like EventBridge
- event router: ingests, filters, and pushes events to known consumers through some other mechanism like SNS
- event consumers: subscribe to receive specific or monitor all events in a stream and act on those they are interested in
- messages vs streaming
  - streaming
    - core entity is the stream: a collection of messages for a specific period of time
    - data remains in the stream for a period of time; each consumer must maintain a pointer
    - messages in streams are reprocessed until success/timeout; you need to build logic into the consumer to skip corrupted messages
  - messaging
    - core entity is the message which varies
    - messages are deleted once consumed
    - configure retries and dead letter queues

### streaming

### messaging

- allows components of distributed systems to communicate with each other

### microservices

- decentralized, evolutionary design
  - each service is paired with the appropriate programming language and technology
  - each component/system can evolve separately
- smart endpoints, dumb pipes
  - there is no enterprise service bus
  - data is not transformed when going through pipes, but each endpoint should transform as needed
- independent products, not projects
- designed for failure
  - services are resilient, redundant, and integration failure
- disposable and transient
  - immutable services and infrastructure with graceful shutdowns
  - start fast, and fail fast and release all file handles
- development and production parity
- microservice architecture: an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API. take a large, complex system and break it down into independent, decoupled services that are easy to manage and extend.
- serverless: abstracts away the infrastructure layer so you can focus on developing your core product

#### service mesh

- networking boundary for communication between services
  - dedicated infrastructure layer using an array of lightweight network proxies deployed as sidecars into containers
  - its all about abstracting away the network configuration from the application code
- service to service communication: east-west
  - dynamically configuring how services are connected
- observability
  - determining how services are performing end-to-end
  - as a request falls through multiple service boundaries, are there bottle necks?
- security & traffic management: authNZ
  - securing comms between services
  - controlling how packets are routed through the nextwork in terms of policies, prioritization and resilience

#### Communication

- client polling: provide a message ID on the initial request, and a status and getResults to check the status/get results of the request
  - perhaps the most wasteful pattern
- webhooks: user defined http callbacks; the client is responsibile for providing the endpoint to be notified when work is complete instead of having to poll
  - trusted: when you own both sides of the integration and can create a secure connection
  - untrusted: when you only own one side of the integration
- websockets: create persistent connections between client and server
  - ideal for streaming/requests that require more than one response

### Data ingestion

- homogeneous: EL; move the data in the same format from source to target
  - foucs is on speeed of transfer, data protection & integrity, and automation for live data
- heterogenous: ETL; move the data in a different format from source to target
  - focus is on transformation of the data to meet destination requirements & running calculations/ML for derived output & new attributes

# copypasta

## Program Patterns

- specific to the internals of a single application, restricted by the application architecture
  - the structural composition of the software program
  - the interactions between software elements
  - during the process of writing software code, edevelopers encounter similar problems multiple times within a roject/compony
    - design patterns give engineers a reusable way to solve recurring problems
- bad design:
  - rigid: hard to change, e.g. due to dependency changes
  - fragile: easy to break, e.g. when making a change bugs cascade
  - immobile: difficult to reuse
- OOP concepts not always relevant to good design principles
  - enheritance: reuse features & behaviors
  - encapsulation: hide & protect data
  - polymorphism: code that works by behavior, and works with types/subtypes with a similar interface
  - abstraction: hide implementation details, and depend instead on declarative interfaces

### design principles

- sets of practices that form the basis of design patterns
- favor composition over inheritance: dont overuse inheritance, composition should also be used to extend behavior
  - identify application components that vary, and separate them from what stays the same
  - HAS-A (a dog has an owner) is better than IS-A (a dog is a animal)
  - objects get new behaviors by consuming (instead of inheriting from at compile time) objects that supply that behavior at runtime
- encapsulate what varies: anything expected to change often should be encapsulated behind some sort of interface
  - its all about letting one part of a system vary independently of others, e.g. moving the brancing logic into a separate module that exports an interface
  - all patterns implement this principle in some form or another
- program to interfaces: program to the most abstract behaviors as possible, allowing implementers of the interface the opportunity to change the specific behaviors as needed
  - one class should not rely on a specific instance of another class
  - objects should rely on interfaces, and shouldnt care which class implements that interface
- loose coupling between objects that interact
  - objects should have little knowledge about implementation details of other objects
  - changes to one should not impact another, if it does, you've got some tight coupling
  - reduces the dependency between components
- SOLID: by Martin Robert Martin
  - S: single responsibility: all about limiting the impact of change
    - an object should only have one reason to change
    - each additional responsibility adds a reason for modifications to be required to the object as the nature of that responsibilty changes in the future
  - O: classes should be open for extension, but closed for modification
    - objects should provide default behavior, but enable consumers to override behaviors when needed
  - L: Liskov substitution: subtypes should be substitutable for their base types
    - i.e. subtypes should adhere to the interfaces of their base types
    - i.e. subtypes behave like their base types
  - I: interface segregation: interfaces should be small and cohesive, and shouldnt include methods they dont use
    - polluted interface: continuing to add new functionality to an interface as it evolves
      - causes unwanted dependencies between consumers of the the interfaces
    - cohesion: how related a class/interfaces methods are to themselves
      - if all consumers of a class/interface use the class/interface the same way, it generally means there is high cohesion
      - else you should subtype the class/interface into additional subtypes, that only implements the behavior with high cohesion
  - D: dependency inversion: high level modules should not depend on low level modules; both should depend on abstractions
    - instead of coupling high level component -> low level component
    - ^ you should instead program to an interface, and have both the high and low components depend on the same interface abstraction
    - ^ the abstraction should not depend on details (implementation), but the implementation (low level component) should depend on the interface
- design by contract: specify preconditions, postconditions & invariants; treat inputs and outputs the same way across implementations

### patterns

- specific strategies for resolving common architecture problems
- adapter
- aggregator
- builder
- chain of responsibility
- decorator
- domain driven development
- facade
- factory: abstracts implementation details of instantiating entities
  - simple factory: encapsulating a complex if statement/similar into a separate module
- interface oriented design
- observer
- repository: abstract implementation details on how to retrieve data; logic for retrieving, validating and translating responses from a data service
- singleton
- strategy
- tryparse: pattern for trying some logic and returning the response, but on error returning the given substitute instead of throwing the exception
- event sourcing: storing events instead of state, enabling you to rehydrate/replay timelines

## APIs

- API-First strategy: where each service within their stack is first and always released as an API

### best practices

- in any API-based application: capture and retry a call whenever possible and handle errors gracefully when a call fails
- before you do anything
  - start with a stable data model before releasing a public API
- structure & design: an API is a user interface for a developer
  - use plural NOUNS (things) and not ACTIONS (http methods) in your endpoint URLs
    - wtf is the difference between goose & geese?
  - use web standards
  - should be explorable via a browser address bar
    - this should also include common search queries
      - package up common sets of conditions into easily accessible endpoints
      - e.g. recently closed could be `/tickets/recentlyclosed` versus a long as fkn filter query param
- http methods have meaning: e.g. a single `/users` endpoint can receive GET, PUT, POST, PUT, PATCH, etc without needing 5 different URIs
  - GET: retrieve thing(s)
  - POST: create thing(s)
  - PUT: update thing(s)
  - PATCH: partially update thing(s)
    - arguable if PATCH should ever be used
    - however PATCH can be used to make an ACTION on a resource appear as a FIELD ona resource
      - e.g. ACTIVATE action could be a PATCH on a resource, even tho the backend data model supports this via other logic (and not an activate field)
  - DELETE: delete thing(s)
  - return useful confirmations from POST, PATCH & PUT requests
- have a single source of documentation
  - available without logging in
  - include copypasta examples
- define a consumable error payload
- effectively use HTTP status codes

### versioning

- have a predictable and publicly available versioning scheme and update schedule
  - CHANGE is coming, everyone knows it, versioning helps manage it
- version via the URL
  - prevents invalid requests from hitting updated endpoints
  - smooth transition to newer versions while sunsetting legacy endpoints
  - ensures browser explorability across versions
  - provides structural stability
- version via HEADER fields
  - useful for specifying minor/patch versions of a major versions
    - e.g. field deprecedations, endpoint changes, etc

### caching

- via response headers
- abcde lol wtf happened to the rest of this section

### security

- use SSL everywhere
  - encrypt communication between parties
  - inhibit eavesdropping/impersonation if authentication credentials are hijacked
  - enables use of access tokens instead of having to sign each API request
- token based authentication
- oauth2 in case delegation is required
- use HARD errors
  - e.g. a client requests a non secure version of an API endpoint
    - an automatic redirect to the SSL version could leak request params over the unencrypted endpoint

### REST

- Representational state transfer (REST) refers to architectures that follow six constraints:
  - Separation of concerns via a client-server model.
  - State is stored entirely on the client and the communication between the client and server is stateless.
  - The client will cache data to improve network efficiency.
  - There is a uniform interface (in the form of an API) between the server and client.
  - As complexity is added into the system, layers are introduced. There may be multiple layers of RESTful components.
  - Follows a code-on-demand pattern, where code can be downloaded on the fly (in our case implemented in Lambda) and changed without having to update clients.
- many features require extended set of options
  - filtering
    - use a unique query param for each field e.g. `poop?field1=yes&field2=no`
  - sorting
    - use a generic queyr param for sorting rules e.g. poop?sort=-field1&field2
      - unary operators -/+ indicate DESC & ASC
  - searching:
    - /users?search=field1...:
      - some resources require SEARCH as a mechanism to filtering and retrieving matches
    - /search?...:
      - a distinct endpoint for searching all resources
  - pagination
    - via link headers
    - via query params
- ability to toggle/specify specific features
  - toggle
    - response envelopes: required by JSONP and other limited http clients
  - specify
    - version: major version in the URL, minor/patch versions via HEADER fields (see stripe/enchant)
    - returned fields: do you need every user field every time?
- autoloading
- http-method-override
  - never on GET requests
- use restful URLs and actions
  - structure your API into logical resources that are manipulated using HTTP methods (CRUD)
    - dont map to your data model 1 to 1
      - this is likely not effective from an API consumer perspective
      - a security risk by revealing the structure to your data modal
      - brittle in the event your data model changes
    - hierarchy
      1. if resource Y is always a child of X, then `/v1/x/:id/y
      2. if resource Y is independent but associated: include an identifier with X where Y can be retrieved with a second API call
      3. if resource Y is independent but always requested with X: see #1 or embed the resource within the call to retrieve X
         - this avoid the second call with approach #2
- only/always use
  - JSON syntax (fk xml) for all HTTP methods
    - some HTTP clients wont be supported, but fk them (unless their paying u)
  - camelCase (fk snake)
  - compression (e.g. gzip) unless in test mode
  - pretty print unless in unless in prod prod

#### HATEOAS

- abcd

### GraphQL

### RPC

#### gRPC

#### tRPC

## system patterns

### command query responsibility segregation (CQRS)

- specific to data problems
- a distinct service for reading from data sources
- a distinct service for writing to data sources

### CQRS and event sourcing combined

- useful for operational & yielding problems

## best practices

- focus on reducing a services blast radius
  - if service A fails, how many other services will be disrupted?
- whether or not you describe your architecture in terms of Tiers, there will always be tiers
  - lower tiered services generally have a bigger blast radius; require the most stability and resilience
  - never let lower tiered services call higher tiered services which may have reduce stability

### Antipatterns

- abstraction for the sake of abstraction increase complexity & cost for no value

## basics

- cascading failure pattern: where a failure at one integration point, cascades to cause failures/disruptions throughtout the application stack in a layered architecture
- architectural style: way of designing processes & building systems to facilitate an end goal, e.g cloud native is an architectural style
- dependency graph:
- service types:
  - data service: connects to a data source within a system
  - business service: builds on top of data services; business domains that aggregate multiple data services to meet business objectives
  - translation service: any abstraction/decoratation/encapsulation of a third party operation under your own interface
  - edge service: serving data to other services based on the consuming services context
- platform: everything within a microservices environment constitutes its platform
  - runtime: virtualization, baremetal services, containers, etc
  - ancillary services: message queues, cache services, oauth, etc; some are first class, others arent
  - operational (devops) components: log & metric aggregators, deployment services, etc
  - diagnostic components: enable you to connect to the runtime env of a microservice and inspect, analyze, diagnose, troubleshoot & improve performance
- business process: logic that consumes one/more data domains to solve an issue
- data pipeline: aggregating data from multiple sources in distinct formats, transforming each to match a single interface, and then storing the transformed data into a single graph/db
  - scheduler: manages retrieval from multiple data sources at different rates, and sending each into the matching data collector
  - data collectors: services geared toward retrieving data from a single data source, serializing and storing the raw data (e.g. in s3)
  - data convertors: convert raw serialized data into a common serialization format, with a defined interface and storing the new formatted data (e.g. back into s3)
  - data processors: take the converted data, and process it for storing into the final db (e.g. a knowledge graph)
- scalability
  - vertical: scale up/down: whatever you have now, but optimized for multi-core, multi-cpu, and high capacity storage devices, usually on the fly
  - horizontal: scale in/out: whatever you have now, but more of it; usually on the fly
- availability
  - low latency and remain highly accesible even in the event of hardware/system/network failures
- fault tolerance: the degree to which a system operates in the presense of failing components
  - load balancing
  - state management
  - redundancy

## Networking

### peer-to-peer networking

- no server exists
- each client is connected to mulitple other clients

### client-server model

- a single server to which all clients connect

### Circuit Breaker

- a service that watches for failures through systems boundaries, and reroutes requests upon detection

### Multi VPC

- VPCs are separated and isolated from one another and can optionally be linked through dedicated connections
- common goals
  - greater flexibility for development, increased security features, and more robust analytical views.

### High Availability

- commonly referred to as eliminating single points of failure
- reducing or managing failures and minimizing downtime through the implementation of redundant components, deployment of parallel components to distribute traffic load, and elimination of single points of failure.

### Hybrid Networking

- at least two independent cloud or on-premises networks communicate with each other

### High performance

- its all about reducing latency: Anything that lengthens the time to get data to the user
  - packet loss
  - jitter: Variations in latency or time delay between packets
  - bandwidth constraints
  - inefficient protocol use
  - physical distance
- When reducing latency, consider
  - physical distance between two nodes
  - quality of routes
  - request origin location in relation to data
  - average packet delay under network cost constraints
  - memory resources
  - traffic patterns and available node resources

### Hub and Spoke

- like a bicycle wheel

### mesh

- like a cobweb

## Storage

- cap theorem: any distributed data store can only provide 2 of three guarantees: consistency, availability, and partition tolerance; since every DB is susciptible to partition failure, its really a choice between consistency and availability
- consistency: every read receives the most recent write/error
- availability: every request receives a (non-error) response, without the guarantee that it contains the most recent write
  - can be overcome via active replication: in the event of failure just switch to the redundant system
- partition tolerance: the system continues to operate despite an arbitrary number of messages being dropped/delayed by the network between nodes
- network partition failure: forces you to either cancel the operation & decrease availabilty but ensure consistency, or proceed with the operation and thus provide availability but risk consistency
- garbage collection
- compaction
- node: generally a unit of storage, e.g. a single db instance including all the software running on the hardware
- cluster: a group of nodes that work together; e.g. a 3 node cluster is the minimum for high availability
- replication: the process of replicating data across nodes ina cluster
- replication factor: The total number of replica Nodes across a given Cluster; the number of copies of a set of data, e.g. RF of 1, means theres 1 copy, RF 2 means there 2 identical copies, etc, generally you want 3
- consistency level: how many nodes must validate a READ/WRITE before the request is considered successful
  - e.g. at least 2 nodes must acknowledge an operation/query/whatever for it to be considered 200
- big data: generally the dataset is so huge it cant be contained in a single node, thus a cluster of nodes are required

### data fabric

### data mesh

- enables collection, integration and analysis of data from disparate systems concurrently in a single location

### data lake

- allows storing yuuuge amounts of raw, structured, and/or unstructured data in a single repository enabling comprehensive analysis from a single location
  - i.e. you push any and everything into a data lake, whether or not the data has a purpose

### data warehouse

- allows storing yuuuge amounts of structured, filtered data that has already been processed for a specific purpose (like data already in use by app/biz)
  - i.e. you push filtered data into a warehouse, for later analysis

### Databases

#### Relational

- relational, structured
- best for online analytical processing

#### NewSql

#### nosql

- hierarchical, unstructured
- best for online transaction processing at scale
- scales out
- main distinctions are data model driven: e.g. rdbms vs nosql vs wide-column etc
- secondary distinctions is according to the cap thereom: usually a choice between C and A, as all are susceptible to P (failures)
  - consistency: will every read receive the most recent write
  - availability: will ever request receive a non-error response (but doesnt have to be the most recent write)
  - partition tolerance: will the system operate in the face of network failures/msg loss

##### key/val

- data model
  - key value pairs: each key has only one value
  - fast queries, no need for a query language

##### document store

- data model
  - data stored in documents of tagged elements (like a row in rdbms)

##### column (oriented)

- long list
- data model
- characteristics
  - wheres SQL stores record by record (row by row), column oriented dbs store column by column
  - improves storage and retrieval performance

##### wide column store

- long list
- data model
  - store data in columns
  - related columns are grouped into tables (column families)
- characteristics
  - supports large numbers of dynamic columns:
  - aka 2 dimensional key value stores

##### graph

- long list
- data model
  - use nodes & edges to store data

##### multi-model

- go for the native multi models, and not the ones enhanced with extensions/plugins/etc (timescale is dope tho)

## monolith

- all services in a system are within a single compile, yield and runtime environment

## N-tier

- stack services based on the flow of data, each stack/tier/layer exists at a different level of abstraction and is responsibile for a specific
  - tier: abstracts services by what they are, instead of what they do, so still some duplication exists over service-oriented architectures
    - the bottom tier(s) generally closer to the data and provides services to the tier above it
  - data flows down from the requester until it reaches a lower tier that can respond to the request
  - a common theme being the hierarchical and layered flow of data, generally unidirectional but in some patterns biderirectional (e.g. server sent events)
- common to think of N-Tier architectures through
  - presentation: UI and pure UI logic; usually the presentation of the business logic solution to external users
  - business: the logic of the problem domain, what does the application do?
  - data: database related stuff
- common goals
  - increase the number of tiers depending on the complexity of the model & types of requests & optmizations required
  - introduce extra layers of defense between attackers and your sensitive resources
    - the most sensitive data should be at the bottom of the stack

### layered

- type of n-tier in which communication between tiers are forced to flow in a single direction, from top to bottom
  - presentation: UI
  - application: abstracts away implemnetation details to interact wiht the business layer
  - business: business logic
  - persistence: abstracts away the implementation details to interact with the database
  - data: database

### microkernel / plugin

- core logic that can be extended via plugins (sidecars), and defines the interface of each plugin
- each plugin/sidecar then implements the contract

### MVC

- extends the multi-tier pattern: divides an application into 3 tiers
  - model: manages application data and data related functionality
    - the central component of the pattern
    - the dynamic data structure of the software application
    - controls the data and logic of the applications
  - view: displays application data and interacts with the user
    - accesses the data in the model and presents it to the user
  - controller: manages the interaction between the model & view layers
    - handles input from the use rand mediates between the model and view
    - listens to external inputs from the view/user and creates appropriate outputs
    - interacts with the model by calling methods to generate appropriate responses
  - communication: the messaging channel connecting the model, view and controller
    - e.g. an event/callback notification system
    - contain state information that is passed between the 3 tiers
      - e.g. an external event from the user may be transmitted to the controller to update the view
- key concepts:
- advantages:
- disadvantages:
- examples:

#### MVP model view presenter

- evolution of MVC
- model: see MVC
- view: see MVC
- presenter: 2 types
  - passive view: all UI logic is in the presenter layer
    - presenter responsible for dynamically creating a static view of the current state of the model
    - and the view is responsible for rendering the static content given to it by the presenter
  - supervising controller: rendering logic is in within the UI, but data transformation logic is within the presenter

#### MVVM: model view view model

- model: business logic & data
- view model: interacts with the model
- view: interacts with the view model
  - two-way databinding between the view data, and the model data

### client-server

- extends the multi-tier pattern: two main components taking roles of service requester (client) and service provider (server)
  - client: initiates & sends service requests to the server
    - the client provides ports for each services it needs
  - server: responds to request from the client potentially fulfilling the request/reasons why it cant
    - the server provides ports for each service it provides
  - communication: generally linked request/reply connectors from client/server M:1
- key concepts
- advantages:
  - central computing of data: generally all files are stored in the central location for the network
- disadvantages:
  - generally need a lot of compute power for the server to handle many requests
- examples:
  - the internet operates on a client-server pattern

### Controller-Responder (master-slave|primary-replica)

- extends the multi-tier pattern consisting of two components the controller and responser(s)
  - controller: distributes work and copies of work-data among identical responders for processing
    - aggregates the responses into a single composite value
    - responsible for writing/storage of data & results from work
    - manages how work is distirbuted among responders
  - responders: processes & returns a slice of work given to it by the controller
    - responsible for processing work and reading data
  - communication:
- key concepts:
- advantages:
  - analytic applications can be read from the responder component without changing the data content of the controller component
  - responders can be taken offline and synced back without data loss
- disadvantages:
  - single point of failure if the controller fails
    - responders can be promoted to controller, but technical deficits exist in the time it takes to transition
- examples:

## service oriented

- beyond tiers, which group by fn (what it is), this groups services by activity (what it does)
- heavily dependent on interface design & contracts and communication architecture patterns
- enterprise service bus: the controller responsible for orchestrating communication between services

### Gateway

- e.g. an API gateway

### microservices

- the extreme in service oriented architectures, with the removal of the enterprise service bus
- each service should be single purpose, stateless, independenty scalable and composable
- involves creating multiple applications (i.e. micro services) that work interdependently
  - although each service can be developed and deployed independently, its functionality is interwove with other microservices
  - each application is free to take on another architecture pattern if complex enough, or use a design pattern
  - messaging:
  - service discovery:
- key concepts
  - separate CI/CD of each service: creates a streamlined delivery pipeline that increases scalability
  - callable via APIs/Events
    - apis: asyncrhonous/synchronous and typically over HTTP
    - events: always asynchronous: useful to think of events as messages
- advantages:
- disadvantages:
- examples

### Microservices

- scoped units of services, that work in unison but scale independently to achieve a goal
  - breaking endpoints into distinct units of work that can be scaled independently
  - focus on data, business and function domains, analyze call patterns and dependency graphs, and determine boundaries between services that need to be scaled independently

#### Presentation Services

- render data for frontend clients in a friendly and common format
  - this layer is prime for a graphql service in a data access layer
- perform simple data transformations/aggregation
- permissions checks
- localization and other external business logic
- data fetching and hydration

#### business services

- shared business logic consumed/sidecarred by many services
- distinct from presentation as this relates to internal business logic

#### data access layer

- in complex situations it may be more appropriate to extract data access from the presentation layer

#### Data services

- encsuplate data sources into a uniform data layer

#### messaging patterns

- Queues
- Messages
- Streams

#### decomposition patterns

- break a component into logical steps, convert each step into a service that can be reused and scaled
- core goal is to make services smaller, thereby more scalable as demand fluxtuates across service boundaries

##### decoupling patterns

- decoupled services: the degree of decoupling has significant implicatins on architecture, performance and scalability
- direct service calls: either sync/async
- producer/consumer: a single producer orchastrates the invocation of multiple consumers, and handles the responses
- pipeline architectures: a form of producer/consumer, however the producer doesnt expect a response from the consumer, but accepts input from the previous stage, does its thing, and passes its output as input to the next stage

##### domain based decomposition

- functional pipelines in a system; create services that fulfill the needs of a particular domain
- largely based on domain-driven design patterns, as that methodology lends itself to decomposing services by domains
- product domain
- inventory domain
- etc etc, really scoped to a particular application/biz/tech context

###### data domain

- services driven by the data itself and focus on serving data thats used by the system, as well as data specific logic
- usually the lowest level of decomposition
  - start with the data model (from the perspective of the outside world), how its needed and how its consumed, what are the actions (not in terms of CRUD/REST, just plainspeak) that need to be performed on the model
  - scope the service contract, dont worry about implementation details just yet
  - define your service boundary and build APIs around your actions
  - define a schema that supports the model (but doesnt have to match 1:1 to the model) and implement your datastore (db)

##### business process based decomposition

- breakdown complex business processes into discrete services each fulfilling a specific role in the overall system
- higher level of service for reusing business logic across other microservices
- enables you to encapsulate related domains, that depend on similar business processes
- business processes should never have direct access to datasources, but instead are given the data they need to operate on
  - this is hard boundary between data domains & business domains
- identify each process you want to expose
- identify the data domain each process requires as input
- define the APIs for each business process
  - business processes always change, and sometimes frequently, so encapsulate the actual business process logic into its own module that can be iterated on separately from the service contract & interface

##### atomic transaction based decomposition

- you build your decomposition model around the atomic transaction at the data domain level
  - these are synchronous blocking calls, be sure this is what you need and understand the performance implications on the system as a whole (this could become a bottleneck)
- domains requiring atomic transactions should be in a single shared database in order to build the atomic service
- when eventual consistency isnt an acceptable model, e.g. within fintech when you need to guarantee ACID transactions across domains
  - atomicity: each statement (CRUD) in a transaction is treated as a single unit; either the entire statement is executed, or none of it is executed
  - consistency: transactions only make changes to tables in predefined, predictable ways
  - isolation: ensures concurrent tansactions dont interfere/effect with one another
  - durability: changes to the data made by successfully executed transaction will be saved, even in the event of system failure
- provide failure domains & rollbacks; blocking API calls until the previous API call is complete, synchronous APIs work best/asynchronous with a guarantee callback mechanism
  - always have clearly defined transactions, especially the conditions which cause commits to be rolled back
- dont depend on distributed transactions, the complexity is far greater than just using a synchronous API

##### strangler pattern

- migrate from a monolithic system into a microservices architecture
- carving/sharding functionality out of a monolith and into a micrservice endpoint
- the most common pattern for moving from monolith to microservice
- start with the monolith, and encapsulate/strangle each dependency, migrating each one-by-one to a microservice endpoint, then deprecate & remove the dependency from the monolith once the microservice is fully implemented
- top down approach: start at the API/service level, and work down to the data domain
- bottom up approach: start the at data domain, and move up to the API/service level

##### sidecar pattern

- promote separation of concerns; all about removing repetitive code from multiple microservices into a single embed that can be utilized by any service
- offload services (e.g. operational/security functions) into distinct components that can be deployed alongside dependent services as runtime dependencies (inversion of control)
- e.g. monitoring, logging, & security etc all lend themselves to this pattern, and can either be deployed as an embedded module (e.g. logging) or as a distinct microservice itself (e.g. oauth)
- this pattern generally shouldnt create more microservices, but instead create child processes to existing microservices

#### integration patterns

- solve orchestration & ingress needs across a system as a whole

##### api gateway pattern

- pattern for external clients communicating with your system
  - if distinct clients have diverging demands/special business logic, use the edge pattern instead
  - works best when all fronted services have similar demand profiles
- clients shouldnt be able to call any/all service, but instead call a single gateway which acts as a reverse proxy to all of the services you provide
- the gateway provides aggregation/buffer/facade/proxy/decoration/oauth/etc to the services behind its fence, and is responsible for mutating, limiting and proxying requests
- keep business logic out of the gateway, while it can be done, there are more appropriate patterns (see process aggregator) that should be responsible
- adhere to strict api version control and ensure all changes are passive (non-aggressive)
- implement clients (service wrappers) as distinct modules for services behind the gateway

##### edge pattern

- client specific API gateways for optimizing cost by creating distinct ingress APIs for clients with distinct business logic & demand profiles
- provides aggregation, consolidation, composition and complexity isolation away from the main API gateway used by the other clients
- directly addresses specific client scaling needs, e.g. if your customer is microsoft/apple, you likely want an edge service scoped to their needs
- identify the client, their needs & constraints, build contracts, interfaces and models
- generally recommended over pure api gateways, unless the cost of the additional hardware/service is too much

##### process aggregator pattern

- aggregate multiple business processes into a distinct flow to solve specific goals
  - usually the processes are dependent, and must be executed consecutively (and not in parallel) and can cause choke points as wait times increase
- whenever you have several business processes that must be called together and have a composite payload (multiple data domains)
- the aggregator interface should provide clients with a single API that subsequently calls each downstream process, assembles the responses from each, and returns a composite payload
- the aggregator is responsible for processing the responses and returning a unified response payload
  - this is the main usecase, else you might as well use an aggrigator at the api-gateway level
  - however, this can cause long blocking calls if too many business processes are aggregated
- determine the business processes
- determine the processing rules (should be encapsulated and an interface presented instead of hardcoding the logic directly in the service)
- design a consolidated model (for the composite payload) based on how the processing rules modify the responses
- design an API for the actions on that model (using either REST/Actions pattern)
- wire the service and implement the internal processing

#### data pattern

##### single service database

- the most common pattern for all data domain based services
- single service, single database
  - perfect when the scalability demands between the db and service are related (usually proportional)
  - as the demand for the service increases, thus the demands on the db, and you can allocate resources effectively by monitoring both
  - thus, each data domain should have its own dedicated datastore in this pattern
  - can potentially isolate the data per region, for a more sophisticated architecture

##### shared service database

- all data domains exist within a single database
  - still, you should break the data domains into schemas/keystores/etc to keep them somewhat distinct at the data level
  - definitely you should still treat the data distinctly at the application code level
  - each user/service that consumes each distinct data domain should also have distinct credentials for accessing those services & data domains, even tho they are really accessing the same data store
    - ensure you have proper segmentation at the data & service level
    - this enables you to move to a single service database in the future with the least amount of friction
- you see this more in enterprises where there are contractual obligations/not enough resources to fully move to a true microservice architecture
- data distribution should be handled by the database, and not by code
  - else synchronization/replication issues will eventually develop if application code is responsible for pushing data across regions/datacenters

##### command query responsbility segregation (CQRS)

- the most complex pattern of all the data patterns
  - if implemented correctly, provides the most benefits for the use cases where its appropriate
    - task based UI operations: complex write queries commit tasks, and complex read queries to support the system state at a given point in time
    - eventually consistency is a must, as usually there are multiple and disparate data domains to be written to and read from
    - event driven models work well, as you can react from triggers & events
    - you must spend significant time in the design phase
  - else is a nightmare to maintain
- data access patterns diverge from traditional CRUD, into multi-model patterns within specific bounded contexts or data domain
  - multi-interface operations, write verus read
    - query interfaces may transform & aggregate the actual data schema to represent the access pattern being modeled
    - write interfaces may inject behavior and other characteristics based on a specific use case being modeled
  - whenever CRUD becomes a bottleneck in complex write & read scenarios

##### asynchronous messaging/eventing pattern

- whenever you have long running transactions/complex workflows and a single blocking API call becomes unfeasable
- some problems/processes cannot be achieved in real time
- service API to trigger event, and events cacade asynchronously
- events can trigger from posting to a messaging queue
- super powerful in distributed systems

#### operational patterns

- how you run run systems, vs how you build it
- helps you answer:
  - what is happening?
  - when is it happening?
  - where it is happening?
  - why is it happening?
  - who is involved?

##### log aggregation patterns

- provide detail behavior of the runtime characteristics of the system
- the source logs need to have a clearly defined model and structure and be consistent across the entire system for effective aggregation and holistic analysis
- in distributed systems, its often usefult to link the logs across systems
- processing logs via aggregation requires each source log to have a consistent taxonomy, similar keys & formats
- log aggregation: each service generally right their own logs thats output for observability, but all logs should end up in a single stream of data
  - aggregation involves retrieving the logged output, parsed, labeled/tagged, and stored in a time-ordered fashion
  - the faster you can aggregate logs, the faster you can diagnose & trouble shoot issues
  - correlation of logs via tracing identifiers requires uniform design across the entire system
    - enables you to recreate call stacks from errant processes
- log indexing: enbales rapid searching, is almost required in high-value production systems

##### metrics aggregation patterns

- understand whats going on at a system level
- ensure the taxonomy is structured across all system components
- dashboard design is critical: both system and user events should be injected into dashboards
  - high-level dashboards: useful for spotting trends across the system as a whole
  - detailed dashboards: useful for drilling down into specific system components to further investigatation
    - especially if you can embed links to the log aggregation system
    - trace alarms on dashboards are useful, that way oncall devs can see why they are being paged visually without having to run queries to figure out where issues are occuring
- ensure you have runbooks for all alarms

##### tracing patterns

- the more call stacks span processes and networks, code traces are less valuable
- you need to implement a higher level trace ID that can be delivered across processes & services
- enables you to recreate the call stack by injecting a trace ID into every call
  - the trace ID should be injected at the edge/entry point to your system, and span all the way down into the data layer, perhaps even stored in the database itself
  - every log message should embed the trace Id through structured logging with common taxonomy
- always use an open-standards approach (usually specify the trace ID in the request header)

##### external configuration

- critical when resources and services are moved across systems
- its main benefits is operational: when you need to reconfigure resources & services, its great to have a single place to manage the runtime environment of microservices
- use tooling that makes external configuration & environment variables easy to find and manipulate
- use consistent naming conventions across services and their configurations
- ensure secrets are kept separate from configurations
- configurations are generally injected into the service, or retrieved as part of the service startup routine
- services should prioritized externalized values over embedded (default) values

##### service discovery

- mechanism for provider services to be found by consuming services in a dynamic runtime where service identifiers/locations/etc can and do change in response to system activity
- a central location should should exist that can be queried to find what services exist to acocmplish each task in the system,
  - as services boot, they should advertise themselves to this datastore, describing their location and what services they offer

### serverless

- not as extreme (implementation wise) as microservices
- currently restricted by the available cloud technology for advanced use-cases
- backend as a service: abstracts security (e.g. authnz), operational (e.g. logging), and database (e.g. RDS) implementation details away for the core application
- function as a service: simple utilities callable over a network

## decentralized

### Peer-to-Peer

- extends the the client-server architecture, utilizing a decentralized system in which peers communicate with each other directly
  - abstraction X...
  - communication:
    - clients can connect to each other for services and connect to the server generally to retrieve the list of available clients
- key concepts:
  - clients are `peers` and share work + responsibilities
- advantages:
- disadvantages:
- examples:
