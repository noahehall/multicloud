# FSx OpenZFS

- fully managed shared file storage built on the OpenZFS file system and accessible through the NFS protocol (v3, v4, v4.1, and v4.2) from Linux, Windows, and macOS compute instances and containers

## my thoughts

## links

- [landing page](https://aws.amazon.com/fsx/openzfs/)

## best practices

### anti patterns

## features

- Power machine learning (ML), financial analytics, and other data-intensive applications with high-IOPS storage.
- file-based web serving and content management applications
- clone application data in seconds, and reduce build times with fast storage for repositories and DevOps solutions, such as Git, Bitbucket, and Jenkins.
- move data residing in on-premises ZFS or other Linux-based file servers to AWS
- supports NFS and SMB protocols for a wide range of application implementations.
- powered by the AWS Graviton family of processors along with the latest AWS disk and networking technologies
- up to 1 million IOPS, with latencies of a few hundred microseconds for your high-performance workloads.

### pricing

- pricing is quoted on a monthly basis, usage is prorated by the second, billed for average usage over a month
- SSD Storage: average amount of storage provisioned per month, measured in gigabyte-months "GB-months,"
- SSD IOPS: three IOPS are included for every GB of SSD storage.
  - optionally provision a higher level of IOPS: pay for average IOPS provisioned above your included rate for the month, measured in "IOPS-months".
- Throughput Capacity:average throughput capacity provisioned per month, measured in “MB/s-months.”
- Backups (incremnetal): storage space consumed
- Data transfer in and out of FSx for OpenZFS across Availability Zones and VPC peering connections.
- Data transfer out of FSx for OpenZFS to other AWS Regions

## basics

- full support for NFS v3, v4.0, v4.1, and v4.2

### ZFS Features

- FSx API

#### Z-Standard compression

- reduce your storage costs for high-performance FSx for OpenZFS file systems and backup storage.

### storage

- multiple data containers (volumes) per file system
- thin provisioning
- user or group storage quotas

#### backups

- point-in-time snapshots: track historical versions of your data and applications, and restore these versions quickly.
- in-place data cloning:
  - created almost instantly
  - consume no additional capacity upon creation
  - data modifications are isolated from your original dataset.
- backups: point-in-time consistent, incremental; copy across Regions
  - daily file-system backups to Amazon S3.
  - backup schedule

### Security

- file & directory-level access control: supports POSIX permissions and POSIX ACLs
- compliance: the longest-running compliance program in the cloud
  - PCI DSS, ISO 9001, 27001, 27017, and 27018
  - SOC 1, 2, and 3
  - HIPAA eligible.

#### encryption

- data at rest: automatically encrypted at rest using keys managed with KMS
- data in transit: 256-bit encryption when accessed from supported Amazon EC2 instance types.

## considerations

## integrations

### EC2

### ECS

### EKS

### VMware Cloud

### WorkSpaces

### AppStream

### VPC

### Transit Gateway

### VPC Peering

### DirectConnect

### VPN

### IAM

- who can administer your file systems, volumes, and backups (create, update, delete, etc.).

### Cloudwatch

### CloudTrail

- monitor and secure API calls

### GuardDuty

- detect and flag suspicious API usage patterns

### CloudFormation

### S3

### KMS
