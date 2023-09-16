# Storage Gateway

- hybrid cloud sotrage solution that connects an on-premise data centers with cloud-based storage and services for storage, backup, management, and security
  - Some workloads cannot easily migrate to the cloud.
  - constantly passing data to and from the cloud is too slow, too resource intensive, or not permitted.
- services
  - hardware appliance: kept in this file with general information
    - all gateway types can use the same storage gateway appliance
  - [file gateway](./storageGateway-fileGateway.md)
  - [tape gateway](./storageGateway-tapeGateway.md)
  - [volume gateway](./storageGateway-volumeGateway.md)

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/?nc=sn&loc=0)
- [hardware appliance](https://aws.amazon.com/storagegateway/hardware-appliance/?nc=sn&loc=2&dn=5)
- [hardware appliance: available regions](https://docs.aws.amazon.com/general/latest/gr/sg.html#sg-hardware-appliance)
- [service endpoints & quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html)
- [api ref](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_Operations.html)
- [pricing](https://aws.amazon.com/storagegateway/pricing/)

## best practices

### anti patterns

## features

- Provide on-premises applications access to cloud-backed storage, low latency
- simplify storage management and reduce costs for key hybrid cloud storage use cases
  - compliance efforts with key capabilities like encryption, audit logging, and write-once, read-many (WORM) storage.
  - moving backups to the cloud
  - using on-premises file shares backed by cloud storage
  - providing low-latency access to locally-cached data in AWS for on-premises applications.
- All Storage Gateway types use standard storage protocols, local caching, and secure and optimized data transfers.

### pricing

- type and amount of storage you use
- the requests you make
- the amount of data transferred out of AWS.

## basics

- Storage Gateway: Connect your on premise data centers to storage services
  - provides public VPC, and Federal Information Processing Standards (FIPS) service endpoints.
  - Your file, database, and backup applications can continue to run without changes.
    - your on-premises applications can access data stored in the cloud via standard storage protocols
    - there's no need to change application code.
  - works as a file share, as a virtual tape library, or as a block storage volume.
- Management and monitoring via gateway console: manage and monitor your Storage Gateway and its associated resources.
- storage protocols: NFS, SMB, iSCSI, or iSCSI VTL
- The primary resource of the Storage Gateway is a gateway.
  - sub-resources: don't exist unless they are associated with a gateway.
  - such as volumes or Internet Small Computer System Interface (iSCSI) targets

### Deployment options for the Storage Gateway appliance

- on premises (required for stored volumes)
  - hardware appliance: see below
  - VM appliance: Download, deploy, and activate the AWS Storage Gateway VM image on any of the supported host platforms.
- in AWS Cloud
  - cached volume: Create an Amazon EC2 instance to deploy your gateway in the AWS Cloud

#### Hardware Appliance

- a physical, standalone, validated server configuration for on-premises deployments
- pre-loaded with Storage Gateway software, and provides all the required CPU, memory, network, and SSD cache resources for creating and configuring File Gateway, Volume Gateway, or Tape Gateway.
- out of the box experience that does not require any additional infrastructure, and is managed from the AWS Console or API.
- use cases: branch offices, research and development departmental workgroups, and laboratory or industrial sites

#### Service Endpoints

- When Storage Gateway is deployed, it must communicate back to the Storage Gateway service for both management and data movement.
- Public endpoint: Storage Gateway connects to a public endpoint over the internet.
- VPC endpoint: Storage Gateway connects to Storage Gateway VPC endpoints over a private connection to AWS
  - via AWS Direct Connect or AWS VPN
- FIPS 140-2 compliant endpoints: Storage Gateway connects to a public endpoint over the internet.
  - Federal Information Processing Standards
  - This endpoint complies with FIPS standards, to further protect sensitive information for regulated workloads in AWS GovCloud (US) Regions.

### Caching

- local gateway appliance maintains a cache of recently written or read data
- use a read-through and write-back cache
  - commits data locally, acknowledges the write operations, and then asynchronously copies data to AWS

### Security

- offers Federal Information Processing Standard (FIPS) 140-2 compliant endpoints in AWS GovCloud (US-East) and AWS GovCloud (US-West).
-

#### Encryption

- in transit
  - between any type of gateway appliance and AWS using SSL
- at rest
  - default Server-Side Encryption with S3-Managed Keys (S3-SSE)
  - or use your own encryption keys through Storage Gateway's integration with KMS

## considerations

## integrations

- integrates with other AWS services for storage, backup, and management while still integrating with on-premises environments.

### cloudwatch

- monitoring

### IAM

- secure access to the service and resources

### KMS

- for encrypting data

### Cloudtrail

- activity logging

### VMware Cloud

- health checks integrated with VMware vSphere High Availability (VMware HA)
- Storage Gateway deployed in a VMware environment on-premises, or in VMware Cloud on AWS,
  - automatically recover from most service interruptions in under 60 seconds

### EventBridge

### VPC

- DirectConnect
