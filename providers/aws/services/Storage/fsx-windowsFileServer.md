# FSx Windows File Server

- fully managed Microsoft Windows file servers, backed by a fully native Windows file system

## my thoughts

## links

- [landing page](https://aws.amazon.com/fsx/windows/?nc=sn&loc=2)

## best practices

### anti patterns

## features

- highly durable and available file systems that can span multiple Availability Zones for Windows applications with full SMB support.
- access from multiple VPCs, AWS accounts, and AWS Regions by using VPC Peering or AWS Transit Gateway.
- data protection with encryption, file access auditing, and automated backups.
- provides a set of administrative and security features, and integrates with Microsoft Active Directory, AWS Microsoft Managed AD and Microsoft SQL Server database workloads without the need for SQL enterprise licensing.
- provides native support for Windows file system features, SMB protocol, built on Windows Server with administrative features that include user file restore, user quotas, and access control lists (ACLs).
- supports all Windows versions, starting from Windows Server 2008 and Windows 7, and current versions of Linux and macOS.
- set up and provision file servers and storage volumes. It can also replicate data, manage failover and failback, and reduce much of the administrative overhead

### pricing

- HDD and SSD storage capacity
- Throughput capacity
- Backups
- Data transfer in and out of FSx for Windows File Server across Availability Zones and VPC peering connections.
- Data transfer out of FSx for Windows File Server to other AWS Regions

## basics

- provides storage of up to 64 TB per file system
- use Distributed File System (DFS) namespaces to create shared common namespaces spanning multiple FSx systems

### high availability

- support for High Availability Microsoft SQL Server deployments
  - deployed across multiple database nodes in a Windows Server Failover Cluster (WSFC)
  - each node having access to shared file storage
  - support for Continuously Available (CA) file shares
- single AZ deployments
- multi AZ deployments

### Security

- Compliance: assessed to meet global and industry security standards. It complies with the following:
  - Payment card industry (PCI) data security standards (DSS)
  - International Organization for Standardization (ISO) 9001, 27001, 27017, and 27018
  - Security Operations Center (SOC) 1, 2, and 3
  - HIPAA eligible

#### authnz

- Identity-based authentication:
  - over SMB through Microsoft Active Directory
  - IAM: users and groups for specific resources.
- Access control and monitoring:
  - supports Windows access control lists
  - network-level access control: VPC security groups

#### Encryption

- data at rest: KMS
  - encrypted automatically before being written to the file system and decrypted automatically as it is read.
- data in transit: using SMB Kerberos session keys, when accessed from compute instances that support SMB protocol 3.0 or newer.

#### Audits

- using Windows Event Logs
- Logs are published to Amazon CloudWatch Logs or streamed to Amazon Kinesis Data Firehose

### storage

- HDD storage: designed for home directories, user and departmental shares, and content management systems.
- SSD storage: designed for the highest-performance and most latency-sensitive workloads,
  - databases, media processing workloads, and data analytics applications.
- Data deduplication: reduce costs associated with redundant data automatically by storing duplicated portions of your dataset only once
- User quotas: monitor and control user-level storage consumption
  - cost allocation across teams
  - limiting storage consumption on a user level.

#### Backups

- automatically takes highly durable, consistent daily backups of your files systems stored in a managed area of Amazon S3
- uses the Volume Shadow Copy Service (VSS) to make your backups file system-consistent
  - file-level restores: undo changes and compare file versions

## considerations

## integrations

### ec2

### ecs

### VMWare Cloud on AWS

### Workspaces

### AppStream

### DirectConnect

### VPC VPN

### VPC Peering

### Transit Gateway

### FSx File Gateway

- For frequently accessed file data

### KMS

### security groups

### Iam

### cloudtrail

### cloudwatch

### kinesis

### s3

### backup

- create scheduled, policy-driven backup plans for your FSx for Windows File Server file systems.

### datasync

- automates and accelerates copying data over the internet or AWS Direct Connect. DataSync copies your files together with file attributes and metadata.
