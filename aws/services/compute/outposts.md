# Outposts

- Run AWS infrastructure and services on premises for a consistent hybrid experience

## my thoughts

## links

- [landing page](https://aws.amazon.com/outposts/?did=ap_card&trk=ap_card)
- [outposts: rack](https://aws.amazon.com/outposts/rack/?refid=ap_card)
- [outposts: server](https://aws.amazon.com/outposts/servers/?refid=ap_card)

## best practices

### anti patterns

## features

- Extend AWS compute, storage, networking, security, and other services on premises for low latency, local data processing, and data residency needs.
- Use the same hardware infrastructure, APIs, tools, and management controls available in the cloud to provide a truly consistent developer and IT operations experience.
- access the full range of AWS services available in the Region to build, manage, and scale your on-premises applications using familiar AWS services and tools.

### pricing

- the form factor: 1/2U rack or the 42U server
- Outpost configurations: what installed locally?
- delivery, installation, and maintenance of the Outpost equipment
- payment plans:
  - All Upfront
  - Partial Upfront
  - No Upfront.

## basics

- a pool of AWS compute and storage capacity deployed at a site
- AWS operates, monitors, and manages this capacity as part of an AWS Region

### Security

#### updated shared responsibility model

- you're new responsibilities
  - physical security of your Outpost racks
  - ensuring consistent networking to the Outpost.

#### Encryption

- at rest
  - encrypted at rest by default on EBS volumes and S3 objects on Outposts.
- in transit
  - between Outposts and the AWS Region through the Service Link.
- deletion
  - All data is deleted when instances are terminated in the same way as in the AWS Region.

### configuration

- choose from a range of pre-validated Outposts configurations
- contact AWS to create a customized configuration designed for your unique application needs.
- Outposts is an extension of the AWS Region.
  - extend your Amazon VPC on premises and connect to a broad range of services available in the AWS Region.
  - you can access all Regional AWS services in your private VPC environment through interface endpoints, gateway endpoints, or their Regional public endpoints.

#### Form Factor

- outposts rack: 42U
  - 80 inches tall, 24 inches wide, and 48 inches deep. Inside are hosts, switches, a network patch panel, a power shelf, and blank panels.
  - installation: assembled and installed by AWS, you just plug it in
  - locally available services: about double whats available in the 1/2U form
  - remotely available services: everything thats regionally available
- outposts server: 1U, 2U
  - rack-mountable servers fit inside 19" width, EIA-310 cabinets.
    - 1U high server is 24” deep, and uses AWS Graviton2 processors.
    - 2U high server is 30” deep and uses 3rd generation Intel Xeon Scalable processors.
  - installation: AWS ships it to you, can be installed by you or third party
    - AWS remotely provisions compute and storage resources
  - locally available services: about half of whats available with the 42U form factor
  - remotely available services: everything thats available in the region
- scale from 1 rack to 96 racks to create pools of compute and storage capacity
- installation
  - AWS installs your Outpost at your customer-managed physical buildingsq
  - meet the facility, networking, and power requirements
  - consists of physical hardware that provides access to the AWS Outposts service. It includes racks, servers, switches, and cabling that AWS owns and manages.

#### Networking

- requires connectivity to an AWS Region.
- service link: a network route that enables communication between your Outpost and its associated AWS Region.
- Each Outpost is an extension of an Availability Zone and its associated Region.

##### VPC

- extend your existing VPC to your Outpost in your on-premises location.
- subnets
  - create subnets in your regional VPC and associated it with your Outpost
    - same as associate subnets with an AZ in a Region
  - Instances in Outpost subnets communicate with other instances in the AWS Region using private IP addresses, all within the same VPC
- local gateway
  - use to connect your Outpost resources with your on-premises network
  - enables low latency connectivity between the Outpost and any local data sources, users, local machinery, and equipment, or local databases.

#### VPN (Outposts Private Connectivity)

- establish a service link VPN connection from your Outposts to the AWS Region over AWS Direct Connect.
- minimizes public internet exposure and removes the need for special firewall configurations.

##### ELB ALB

- automatically distribute incoming HTTP and HTTPS traffic across multiple targets on your Outposts.
  - Targets include as Amazon EC2 instances, containers, and IP addresses.
- fully managed, operates in a single subnet, and scales automatically up to the capacity available on the Outposts rack.

##### AppMesh

#### Storage

##### ebs

- offers local instance storage and EBS gp2 volumes for persistent block storage.
- gp2 volumes for boot or data volumes
- attach/detach EBS volumes to EC2 instances on your Outpost.
- EBS features
  - offered in tiers of 11 TB, 33 TB, and 55 TB.
  - snapshot and restore capabilities.
  - volume resizing without any performance impact.
  - volumes and snapshots on Outposts are fully encrypted by default
- snapshots: point-in-time copy of your EBS volumes
  - default snapshots are stored on Amazon S3 in the same AWS Region.
  - Local snapshots on Outposts require that you provision your Outpost with S3.
    - on-premises data protection solution for residency requirements
    - migrate workloads from any source directly onto Outposts, or from one Outpost to another

##### s3 on outposts

- [s3 on outposts](../Storage/s3-outposts.md)

#### Databases

##### RDS

##### Elasticache redis/memcached

#### Compute

##### EC2

- the latest generation Intel powered EC2 instance types with or without local instance storage.
- General purpose: M5/M5d; a balance of compute, memory, and network resources
- Compute optimized:C5/C5d; compute-intensive workloads
- Memory-optimized: R5/R5d; workloads that process large datasets in memory & memory-intensive applications
- Graphics optimized: G4dn; machine learning inference and graphics-intensive workloads
- I/O optimized: I3en; provides dense Non-Volatile Memory Express (NVMe) SSD instance storage optimized for low latency, high random I/O performance, high sequential disk throughput.
- also supports C6g, M6g, and R6g
- [builds ontop of ec2 nitro](../compute/ec2-nitro.md)

##### ECS

##### EKS

#### Analytics

##### EMR

## considerations

## integrations
