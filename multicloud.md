# multicloud

- likely want to continue splitting this out into multiple files

## links

- [cap theorem](https://en.wikipedia.org/wiki/CAP_theorem)
- [database transactions](https://en.wikipedia.org/wiki/Database_transaction)
- [gcp: what is multicloud](https://cloud.google.com/learn/what-is-multicloud)
- [learning domain driven design](https://dddcommunity.org/learning-ddd/)
- [microservice architecture patterns and best practices](http://microservices.io/index.html)
- [networking: Address Allocation for Private Internets](http://www.faqs.org/rfcs/rfc1918.html)
- [networking: intro](https://web.stanford.edu/class/cs101/network-1-introduction.html)
- [networking: ip address & cidr range visualizer](https://cidr.xyz/)
- [networking: understanding ip addressing](https://www.ripe.net/about-us/press-centre/understanding-ip-addressing)
- [noisy neighbor antipattern](https://learn.microsoft.com/en-us/azure/architecture/antipatterns/noisy-neighbor/noisy-neighbor)
- [OWASP quick reference](https://www.owasp.org/images/0/08/OWASP_SCP_Quick_Reference_Guide_v2.pdf)
- [serverless framework](https://github.com/serverless/serverless)
- [shared nothing architecture](https://en.wikipedia.org/wiki/Shared-nothing_architecture)
- [state of devops 2021 (PDF)](https://services.google.com/fh/files/misc/state-of-devops-2021.pdf)
- [TCO: serverless vs traditional clouds](https://pages.awscloud.com/NAMER-field-GC-Deloitte-TCO-whitepaper-2019-learn.html)

### ISO standards

- [popular ISO standards](https://www.iso.org/popular-standards.html)
- [iso 31000 risk management framework](https://www.iso.org/iso-31000-risk-management.html/)

### tools

- [bpftrace: inspect syscalls](https://github.com/iovisor/bpftrace)

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

- opensource: conflicts with responsibilities of proprietary cloudnative services
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

## Storage

- Network File System: FNS; generally linux based systems
- Server Message Block: SMB; generally windows
- SAN: storage area networks
- IOPS: input/output operations per second
- R/W patterns
  - Worm: write once, read many: for data with heavy reads

### Consideration

- characteristics that will lead you toward the best storage solution
  - Type of access method (block, file, or object)
    - storage access protocols: applications are often developed based on specific operating systems
    - How much storage capacity is required for each type
  - Patterns of access (random or sequential)
  - Required throughput
    - How should you configure your volumes?
    - What are the recommended block sizes? What are the cache sizes?
    - Frequency of access (online, offline, archival)
    - Frequency of update (WORM, dynamic)
  - Availability and durability constraints
    - Is there temporary data that does not need to persist?
    - Does data need to be shared? which services need to share the data? How does it need to be shared?
- optimization: identify constraints & service options for improvement
  - understand the performance profile for each of your workloads
  - performance analysis to measure input/output operations per second (IOPS), throughput, and other variables
  - metrics improvement strategies as part of a data-driven approach, benchmarking/load testing
    - Define your storage performance requirements
    - Identify your workload’s most important storage performance metrics
- Q/A
  - storage-data-application workflow
    - New/Existing
    - requirements: interopability with other systems, configuration, retention, compliance, business, access patterns
    - Use case: high performance computing, databases, static content, backup/retrieval heavy, read vs write heavy, authnz/data sharing
  - Storage
    - location: remote, edge, hybrid, regional
    - type: block/file/object/combination
    - performance: iops/throughput/latency/transactions
  - Data
    - access protocols: rest/protobuff/nfs/ftp/smb
    - transfer performance: over the wire/physical/between services/redundancy
    - protection: backups, retention, compliance, longterm archival, point-in-time snapshots
- workload considerations
  - IOPS-intensive (SSD) or throughput-intensive (HDD)
  - latency sensitivity:
    - high performance IOPS: very low and sub-millisecond to single-digit millisecond latency is needed
    - midgrade IOPS: single-digit to low two-digit latency is tolerable
    - not latency sensitive: go with the cheapest option available

#### storage performance

- latency: aka delay; amount of time between making a request to the storage system and receiving the response.
  - How the storage is connected to the compute system
- Input/output operations per second: IOPS; a statistical storage measurement of the number of input/output (I/O) operations that can be performed per second
  - used to measure the number of operations at a given type of workload and operation size can occur per second
- throughput: statistical storage measurement used to measure the performance associated with reading large sequential data files.
  - Large files, such as video files, must be read from beginning to end.
  - operations are measured in megabytes per second (MB/s).

### storage types

#### block storage

- raw storage in which the hardware storage device or drive is a disk or volume that is formatted and attached to the compute system for use
- splits data into chunks (aka blocks; each with distinct addresses) and stores them on disk, subject to fragmentation over time
- its more efficient when changing a piece of the data, as only the chunk needs to be updated
- R/W pattern: WORM
- examples: HDDs, SSDs, NVMe, SAN systems
- use cases: transactional workloads, containers, virtual machines, i/o intensive apps, operating systems, databases, big data analytics engines
  - used by the operating system or an application that has the capabilities to manage block storage directly
- architecture
  - the block storage: physically/logically attached to the compute system
  - the compute system:
  - the OS/application: runs on the the compute system, manages the block storage
    - recognizes the block storage as available and formats or makes the block storage avaible for use
      - an app running on the compute system can act as the managing entity rather than the OS

##### block data components

- block size: select the block size that best meets your use case
  - Block size flexibility is a fundamental differentiator for block storage
- metadata management: information that the operating system and users need to identify and track the data.
  - e.g. resource type, permissions, and the time and way it was created.
- read/write activity: controls in place to manage access to the data.
  - uses metadata permissions to control who can access, modify, or delete the data.
  - can also reference external sources such as Microsoft Active Directory or Lightweight Directory Access Protocol (LDAP) to determine these permissions.
  - manages the caching or read activity
  - determines how writes are cached and staged before writing the blocks and which blocks to write to.
- locking control: manages data integrity when data is being modified or deleted
  - file-level locking: applying a lock on the entire data file
  - block-level locking: specific portions or blocks being modified.
- volumes: a logical storage construct that can be created from a single drive or using multiple drives

#### file storage

- built on top of block storage, typically serving as a file share or file server.
  - created using an operating system that formats and manages the reading and writing of data to the block storage devices
- treats data as atomic units (e.g. a file) but also organized in a tree structure, like your filesystem
  - ideal when you require centralized access that must be easily shared and managed by multiple host computers
  - if changing a piece of data, you need to replace the entire file
- use cases: web servers, analytics, media, file systems
- storage protocols: enable users to use the servers' resources or share, open, and edit files.
  - network protocols: enable users to communicate with remote computers and servers.

##### Server Message Block: Storage Protocol

- network protocol

##### Network File System: Storage Protocol

- network protocol

#### object storage

- built on top of block storage: created using an operating system that formats and manages the reading and writing of data to the block storage devices
- treats data as atomic units (e.g. a file) and stores it on disk in a flat hierarchy, not subject to fragmentation over time
  - if changing a piece of the data, you need to replace the entire object
- Unlike file storage, object storage does not differentiate between types of data
  - method of storing files in a flat address space based on attributes and metadata.
- uses cases: data archiving, backup and recovery, rich media; systems requiring file versioning, file tracking, and file retention.

### Onpremise capacity calculations

- raw: what you pay for and used calculate the operating costs and data center requirements.
  - The net usable capacity will vary by manufacturer and by individual system.
- formatted: raw capacity - hardware failure protection overhead, drive formatting, and operating system overhead.
  - Hardware failure protection overhead: aka hardware or software redundant array of independent disks (RAID)
    - protects the data if hardware or a drive fails by creating checksum protection for the data
    - Depending on the protection level, this can amount 15%–50% overhead
  - Formatting and operating system overhead: The operating system is then added to the system, which further reduces the available capacity 1%–5%
- allocated: formatted capacity - data protection services, such as snapshots, and add space for performance overhead
  - Snapshots can consume more space than your actual data
  - systems require additional space for operation overhead, especially for write operations.
- actual: business requirements + allocated:

## analytics

- key domains

  - customer experience
  - performance over time: system, costs, etc
  - trends
  - troubleshooting and remediation: identification, isolation and resolution
  - learning and improvement: detecting and preventing problems

### monitoring

- all about metrics, logging and tracing
  - processes must be in place to capture logs and other useful artifacts
  - captured logs and artifacts must be stored in a durable, searchable location
  - alerts and automation
- monitoring: The act of collecting, analyzing, and using data to make decisions or answer questions about your IT resources and systems
- monitoring tools: collects data generated by systems
- metrics: a datapoint consisting of a name and value
- dimensions: qualities that describe the context of a metric, consisting of a name and value
- statistics: metrics monitored over time
- logs: collect and aggregate files fomr resources and filterout actionable insights from background noise
- tracing: follow the path of a request as it passes through different services
  - investigate how apps and their underlying services are performing
  - important for troubleshooting the root cause of performance issues and errors

#### Network Monitoring and Troubleshooting

- monitor the availability, uptime, operation, and performance of complex networks
- reduce the mean time to repair and recover and solve real-time network performance issues.
- key tasks:
  - Tracking and analyzing network components and the connections between them
  - Surveilling different data layers, network endpoints, and links
  - the health and performance of network interfaces for their faults helps to diagnose, optimize, and manage various network resources
    - provide historical data and establish a baseline
    - important for forensic analysis to identify the root cause after incidents.
  - Data in the form of tables, charts, graphs, dashboards, and reports.

##### general process

- monitoring devices and network components
  - identify performance metrics to be monitored
  - deterime the monitoring interval:
    - the frequency at which network devices are polled to identify performance and availability status
  - choosing the right protocols for devices & network components
    - SNMP: simple network management protocol
    - HTTP: hyper text trasnfer protocol
    - TCP: transmission control protocol
    - IP: internet protocol
    - ICMP: internet control message protocol
    - WMI: windows management instrumentation
  - set proactive thresholds
  - alert, alert, alert!Network Monitoring and Troubleshooting
- monitoring applications, services and resources: use a tool that will get you 90% there
  - record performance-related metrics appropriate for the service: e.g. db transactions, slow queries, i/o latency, http request throughput, service latency, etc
    - Identify metrics that matter for your workload and record them.
    - Identify the target, measurement approach, and priority to build alarms and notifications to proactively address performance-related issues.
  - analyze metrics when events/incidents occur
    - monitoring dashboards or reports to understand and diagnose the impact.
    - write use cases for your architecture, include performance requirements and incident responses.
  - establish KPIs to measure workload performance
    - Identify the key performance indicators (KPIs) that indicate whether the workload is performing as intended.
    - Document the performance experience of customers, and use these requirements to establish your KPIs
  - use monitoring to generate alarm-based notifications
    - using your KPIs, a monitoring system should automatically alert when measurements are outside of the baseline.
  - review metrics at regular intervals
    - review the metrics collected to identify which metrics were key in addressing issues
    - Also ask which additional metrics would help to identify, address, or prevent issues.
  - monitor and alarm proactively
    - Use KPIs, combined with monitoring and alerting systems, to proactively address performance-related issues.

##### diagnostic tools

- always need to be compared against historical baseline of network performance
- ping: some service providers have ping (ICMP echo packets) disabled by default, or cant be enabled at all
- traceroute: uses successive echo packets to display the path to the destination and the response time of each hop.
- Speedtest: useful in evaluating the performance of your internet access.
- packet analyzers: aka packet sniffers; logs each packet it intercepts, decodes the packet, and presents the values of the various fields within the packet for examination.
- benching tools: measure throughput and bandwidth;
  - iperf/iperf3: tools for active measurements of the maximum achievable bandwidth on IP networks
    - supports tuning of various parameters related to timing, buffers, and protocols (TCP, UDP, SCTP with IPv4 and IPv6)
  - extrahop: monitoring solution for security, network performance, and the cloud
  - netperf: a CLI tool similar to iPerf that measures throughput and benchmarking speeds.

##### common networking metrics

- bandwidth capacity: the maximum data transmission rate possible on a network
  - measures the theoretical limit of data transfer
  - For optimal network operations, you want to get as close to your maximum bandwidth as possible without reaching critical levels
  - indicates that your network is sending as much data as it can within a period of time, but isn’t being overloaded.
- throughput: measures your network’s actual data transmission rate
  - measures the units such as megabyte or gigabyte per second of data packets that are successfully being sent.
  - a high bandwidth connection but low throughput, that's an indicator of an underlying problem
- latency: delay between requesting data and when that data is finished being delivered.
  - Consistent delays or odd spikes in delay time indicate a major performance issue
- packet loss: examines how many data packets are dropped during data transmissions on your network
  - The more data packets that are lost, the longer it takes for a data request to be fulfilled
  - A network’s TCP interprets when packets are dropped and takes steps to ensure that data packets can still be transmitted;
- retransmission: is when packets are lost, The network needs to retransmit them to complete a data request.
  - retransmission rate lets your enterprise know how often packets are being dropped, which is an indication of congestion on your network.
  - analyze retransmission delay (or the time it takes for a dropped packet to be retransmitted) to understand how long it takes your network to recover from packet loss.
- availability: i.e. uptime, the percentage of time the network is available.
  - can never guarantee 100 percent availability, but you want to be aware of any downtime that happens on your network that you weren’t expecting
- connectivity: whether the connections between the nodes on your network are working properly
  - jitter: a variation in delay or disruption that occurs while data packets travel across the network.
  - congestion: occurs when network devices are unable to send the equivalent amount of traffic they receive.
- response times: measures the time it takes for a server to respond to a data request with application data

##### fault monitoring:

- when a system polls registered devices at established intervals to verify if they respond
- this is the simplest form of network monitoring

##### performance monitoring:

- collects detailed information to help determine possible reasons for poor network performance
- simulate real user traffic
- establish a network performance baseline measurement for each segment of a network
- deploy monitoring agents to constantly monitor network performance
- automated alerts when thresholds are breached

##### capacity monitoring

- monitor the users, applications, and other services on your network to see if one or many are draining the network

##### Application Performance Optimization

- monitoring and troubleshooting of performance issues with applications
- permits applications to evaluate the network API communication from the application perspective

##### Automated Alerts

- When there are issues, you should be alerted immediately, either through:
  - on-screen displays
  - Text and emails automatically generated by the network monitoring solution
- every alert should contain
  - when a problem occured and which threshold is being approached/breached
  - information to identify the device

### observability

- the extent to which a system can be monitored
- you observe a system through metrics, logs and traces (see monitoring)
- considerations
  - storage costs
  - data overload
  - ensuring your system is outputting the correct data

## Cloud Financial Management (CFM)

- set of activities that enable organizations to measure, optimize and plan costs as you grow your adoption of cloud services
- outcomes: CFM goals
  - reduce unit costs as you scale
  - reinvest wasteful spend and increase business agility
  - improve financial predictability
  - establish cost aware behaviors and culture
- FinOps: cross functional finance and technology team

### capabilities

- guage your organizations maturity in the following domains
- maturity level: 1 novice -> 5 expert
  - only the expert level is listed, fk the rest

#### ownership and accountability

- who is responsible for driving CFM across the entire organization
  - identify an owner (SME) or establish a cross-functional team with finance, technology and others
  - define common goals and targets
- expert level
  - active owner with goals graded against targets
  - consistent executive sponsorship
  - programmatic CFM activities

#### finance and tech cooperation

- establish strong partnerships between finance and tech
  - financial stakeholders become more cloud & technology savvy
    - understand how technology is being used to drive growth
  - technology stakeholders become more finance savvy
    - participating in finance activities like spend reviews, forecasting exercises and budget discussions
  - initiate short, periodic meetings between all stakeholders
    - weekly meetings, weekly email summarizing activities and latest metrics, etc
  - spread awareness and publicize CFM activities across the organization
  - Share knowledge across members of the CFM team
- expert level
  - formal partnership between finance and technology
  - finance org utilize each cloud providers financial toolset from an operational perspective
    - they can pull latest metrics and stats relevant to finance activities in real time
  - tech org is aware of and participates in finance organization activities
    - tech supports finance initiatives
  - finance and/or tech orgs educate external audiences
    - the CFM team can present their activities in detail to relevant external stakeholders

#### cost allocation

- mechanisms to allocate consumption of cloud services back to the actual consumer of the cloud
  - identify the most important dimensions for your specific business
    - e.g. categorizing based on teams, lines of business, cost center, etc
  - define a strategy for accounts and tags
    - atleast separate accounts by env (prod, dev, etc)
    - publish, evangelize and govern resource tagging across accounts
  - allocate shared costs
    - e.g. containers, clusters, storage, etc can all be allocated based on resource consumption
- expert level
  - account segmentation strategy and tagging policy exists and enforced
  - full account and tag governance
  - cost allocations for discounts and shared resources
    - there are always cross-cutting concerns that spans accounts, but still need to be allocated effectively
  - special tag handling

#### cost visibility

- visibility into cloud spend
  - define KPIs based on unit costs of cloud resources (e.g. X per hour, Y per transaction)
    - set KPI goals and targets, and automate alarms to detect anomalies
  - enable stakeholders: across finance and organization teams provide reports in a digestable manner
    - some people need details, other people need summaries
    - provide training to enable usage of reporting tools
  - can trace spend back to resource usage across different dimensions
  - tools and processes in place to view and analyze spend
  - regularly report on cloud activities how they relate to KPIs to stakeholder groups
  - data is used to influence technology group activities
- expert level
  - custom dashboards utilizing each cloud providers APIs to generate reports specifically for cost and usage metrics
  - stakeholders activtely use the dashboards
  - efficiency KPIs drive decision making related to cloud adoption and consumption activities

#### cost optimization

- reduce existing costs and avoid unnecessary costs through upfront workload design and embedding cost optimization in all operational processes
  - effective consumption planning
    - avoid on-demand prices by utilizing purchase plans, commitment based discounts and other provider cost models
  - right-size your resources: you must know your workload
  - use automation to clean up waste: shutdown unused/idle resources
  - prioritize high ROI investments: architect for cost optimization
  - design fault tolerant systems and use cloud services that scale horizontally
- expert level
  - commitment based discounts, purchase plans and other non ondemand cost models are utilized for prod, dev and other workloads
  - proactive optimization through design, architecture and resource selection
    - not only identifying what you can do, but actively implementing strategies to reduce costs across the organization
    - you must bake cost optimization into the design phase of the tech organization
  - continuous and increased level of automated optimization

#### forecasting

- mechanisms for forecasting future costs in order to improve the business and financial predictability
  - use trend based forecasting for consistant usage
  - use driver based forecasting to capture business changes
  - track actuals vs forecast and understand variance drivers
  - establish a period variance mitigation process and publish results to key stakeholders
    - root cause analysis
    - named stakeholders
    - publish results to the org
- expert level
  - trend and driver based forecasting
  - recurring detailed variance analysis
  - standard operating procedure or well defined playbook for mitigating variances
  - high forecasting accuracy

### 4 pillars of CFM

- see: measurement and accountability
- save: cost optimization
- plan: planning and forecasting
- run: financial operations

#### Measurement and Accountability (see)

- activities that establish cost and visibility to ensure transparency and accountability for spend

##### account and tagging strategy

##### cost reporting and monitoring processes

##### cost show/chargeback

##### efficiency/value KPIs

#### Cost Optimization (save)

- activities that ensure your organization pays only for resources it needs

##### cost aware architecture

###### design and service selection

##### match capacity with demand

##### purchase model selection

##### identifying waste resources

#### Planning and Forecasting (plan)

- activities that allow your organization to better undertand costs associated with future cloud workloads

##### budgeting & forcasting

###### variable cloud usage

##### POC based cost estimation

##### business case

###### value articulation

##### strategic fit

#### Financial Operations (run)

- activites that enable your organization to scale CFM

##### secure executive sponsorship

##### Finance + Tech cooporation

- partnership between finance and technology organizations

##### People, Governance, Tools

- investing in the right things

##### Accomplishments

- celebrating, rewarding and promoting good practices
