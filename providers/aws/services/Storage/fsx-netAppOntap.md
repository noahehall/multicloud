# FSx NetApp ONTAP

- designed to provide both NetApp block and file storage. feature-rich, fast, and flexible shared file storage that’s broadly accessible from Linux, Windows, and macOS compute instances

## my thoughts

## links

- [landing page](https://aws.amazon.com/fsx/netapp-ontap/)

## best practices

### anti patterns

## features

- high-performance storage and QoS controls, and instantly snapshot and clone datasets regardless of size.
- protect against ransomware and other attacks with advanced security features.
- data retention and disaster recovery with simple and secure backup, archive, and replication from on-premises file servers or across AWS Regions.

### pricing

- SSD Storage: average amount of storage provisioned per month, measured in gigabyte-months "GB-Months".
- SSD IOPS: 3 IOPS are included for every GB of SSD storage
  - optionally provision a higher level of IOPS: pay for the average IOPS provisioned, measured in "IOPS-Months".
- Capacity Pool usage: pay requests costs (per read and write operation) whenever the data is accessed.
- Throughput Capacity: pay for the average throughput capacity provisioned per month, measured in “MBps-months”.
- Backups (incremental): average amount of backup storage consumed per month, measured in “GB-months”.
- SnapLock licensing: average amount of storage space consumed in SnapLock volumes per month, measured in “GB-months".

## basics

### storage

- thousands of simultaneous clients running in EC2, ECS, EKS, VMware Cloud, WorkSpaces, and AppStream 2.0 instances.
- storage thin provisioning: each volume only consumes storage capacity from your file system for the data stored in the volume.
  - set the size for each volume to limit the amount of data that a volume can store
    - apply user / group quotas
- throughput capacity
- IOPS provisioning:

#### file

- access to shared file storage over all versions of NFS and SMB protocols
- supports multi-protocol access (i.e. concurrent NFS and SMB access) to the same data

#### block

- access protocol: iSCSI

#### elastic Capacity Pool Tiering

- elastic storage that’s cost-optimized for infrequently-accessed data.
- automatically transitions data from primary SSD storage to capacity pool storage based on access patterns
- primary storage
  - high-performance SSD storage that’s purpose-built for the active portion of your data set.
- capacity pool storage
  - automatically grows and shrinks as you tier data to it

### Migrations

- NetApp SnapMirror replication: migrate from on-premises ONTAP deployments into the AWS
  - replicate files, file metadata, and file system configuration

### High availability

#### multi-az deployments

- include an active and standby file server in separate AZs
- automatically fails over
- synchronously replicated

### NetAppp features

#### Administration

- AWS-native and NetApp management tools: set up, manage, and monitor your file systems
  - AWS Console, AWS CLI, AWS SDK
  - NetApp Cloud Manager
  - ONTAP’s REST API.
- Compression and deduplication: automatically reduce the storage consumption on your file system storage and your file system backups

#### SnapCenter

- SnapShots: application-consistent snapshots
- undo changes and compare file versions; restoring individual files and folders

#### SnapMirror

- replicate data between two ONTAP file systems across AWS regions
- replication with a Recovery Point Objective (RPO) as low as 5 minutes
- Recovery Time Objective (RTO) in single-digit minutes

#### FlexClone

- create a clone of the volumes in your file system instantaneously
  - a point-in-time, writable copy of its parent volume that shares data blocks with its parent
  - the clone consumes no storage for data shared with its parent and takes up minimal incremental space

#### Global File Cache

- On-premises caching: low-latency access for your most frequently-read data to on-premises clients and workstations.

#### FlexCache

- configure FSx for ONTAP as an in-cloud cache for your on-premises data
- provides low-latency access to your on-premises data sets from AWS compute instances.

#### FPolicy

- through AWS Partner solutions to monitor for file access events.

#### VScan

- through AWS Partner antivirus applications to automatically scan new files as they are written to your file system.

### Security

- optionally use ONTAP export policies to configure which clients can read and write to the volumes in your file system.
- VPC Security groups & NACLs
- IAM: resource level permissions
- Microsft AD: supports identity-based authentication over NFS or SMB if you join your file system to an AD
  - users can then use their existing AD-based user identities to authenticate themselves.
- global and industry security standards.
- PCI DSS, ISO 9001, 27001, 27017, and 27018), and SOC 1, 2, and 3,
- HIPAA eligible.

#### Auditing

- enable audit event logging using ONTAP’s native audit logging capabilities
- ONTAP records file access events to a log file that you specify in your file system. You can read that log file using applications such as Windows Event Viewer.

#### Encryption

- data at rest
  - automatically encrypted at rest using keys managed with KMS
  - Data is automatically encrypted before being written to the file system, and automatically decrypted as it is read.
- data in transit
  - supports Kerberos-based encryption with AD
  - TLS on all connections

## considerations

## integrations

### IAM

### workspaces

### KMS

### cloudtrail

- integrates with CloudTrail to monitor and log administrative actions made in the NetApp ONTAP console, API, and CLI.

### VPC
