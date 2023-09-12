# elastic file system (EFS)

- serverless, cloud-native network file system for Linux and any AWS compute service

## my thoughts

## links

- [landing page](https://aws.amazon.com/efs/?did=ap_card&trk=ap_card)
- [total cost of ownership example](https://aws.amazon.com/efs/tco/)

## best practices

### anti patterns

## features

- persistent storage for content management systems, machine learning and big data analytics workloads
- create and configure shared file systems for AWS compute services
- eliminate capacity planning: designed to scale on demand to petabytes without disrupting applications, growing and shrinking automatically
- 11 9s durability and up to 49s of availability
- automate deletion/movement of infrequently accessed files
- Supports authentication, authorization, and encryption capabilities to help you meet your security and compliance requirements.

### pricing

- pay for total data stored
- read and write access to data
  - stored in Infrequent Access storage classes
  - using Elastic Throughput
  - using Provisioned Throughput

## terms

## basics

### storage classes

- multi-az; highest levels of durability and availabilityt
  - standard:
  - standard IA: infrequent access
- single AZ; cheapest
  - one zone:
  - one zone IA: infrequent access

### performance modes

- General Purpose: for latency-sensitive use cases, such as web serving environments, content management systems, home directories, and general file serving
- Max I/O: scale to higher levels of aggregate throughput and IOPS. The tradeoff is slightly higher latencies for file metadata operations.

### throughput modes

- Bursting Throughput: scales as your file system grows
- Provisioned Throughput: specify the throughput of your file system independent of the amount of data stored.

### High Availability

- Amazon EFS file system object (directory, file, and link) is redundantly stored across multiple Availability Zones for file systems using Standard storage classes.

### EFS lifecycle management

- Lifecycle management reduces storage costs by up to 92 percent.
- age-off policy of 7,14, 30, 60, or 90 days
- files automatically move from Amazon EFS Standard storage to EFS Standard-IA storage, or from Amazon EFS One Zone storage to EFS One Zone-IA storage.

### EFS Replication

- abcd

### Security

- uses a traditional file permissions model, file locking, and hierarchical directory structure using the NFSv4 protocol
  - on-premises servers can access file systems using AWS Direct Connect or AWS Virtual Private Network (AWS VPN).
- control network access to your file systems by using VPC security group rules
- control application access using IAM policies and Amazon EFS access points.

#### Access points

#### encryption

- Data at rest is transparently encrypted by using encryption keys managed by the KMS
- data in transit uses TLS to secure network traffic without having to modify your applications.

## considerations

## integrations

### IAM

### security groups

### kms

### EKS

- integrates with k8s via an EFS CSI driver
- use cases
  - application workloads requiring replicas to span across worker nodes and access the same application data
- general workflow
  - a k8s storage class backed by EFS will direct the EFS CSI driver to make calls to the appropriate aws apis to create an access point to a preexisting file system
  - when a peristent volume claim (PVC) is created
  - a dynamically provisioned peristent volume (PV) will use the access point for access to EFS file system then bind to the PVC

### Lambda

### ECS

### DataSync

### Backup

### Transfer
