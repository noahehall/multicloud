# FSx Lustre

- parallel file system designed for compute-intensive applications requiring speed and high throughput

## my thoughts

## links

- [landing page](https://aws.amazon.com/fsx/lustre/)

## best practices

### anti patterns

## features

- Accelerate compute workloads with shared storage that provides sub-millisecond latencies, up to hundreds of GBs/s of throughput, and millions of IOPS.
- Support thousands of compute instances running complex analytics workloads on petabytes of data.
- Reduce and monitor storage costs with automatic data compression and storage quotas.
- import the dataset directly from on premises to FSx for Lustre over AWS Direct Connect or a virtual private network (VPN) connection.
- uses cases: machine learning, high performance computing (HPC), video processing, and financial modeling.

### pricing

- File system storage: average amount of storage provisioned for your file systems per month, measured in gigabyte-months "GB-months,
- backups (incremental): the amount of backup data stored, measured in gigabyte-months (GB-months)
- Data transfer:
  - region specific data rates
    - i/o from Amazon Sx across AZs
    - VPC Peering connections in the same AWS Region
  - inter-region data rates
    - data out from FSx to another Region
    - backups across Regions

## basics

- POSIX-compliant: native file system interface works with any Linux operating system
  - read-after-write consistency
  - file locking.
- SSD storage: latency-sensitive workloads or workloads requiring the highest levels of IOPS/throughput.
- HDD storage: throughput-focused workloads that aren’t latency-sensitive
  - optional SSD cache: automatically places most frequently read data on SSD
    - the cache size is 20% of file system size

### Scratch file systems

- Scratch file systems: temporary storage and shorter-term processing of data. Data is not replicated and does not persist if a file server fails.

### persistent file systems

- Persistent file systems: longer-term storage and workloads. Data is replicated and file servers are replaced if they fail.

### Security

- integrated into other AWS services for security and compliance.
- compliant eligible with the following:
  - Payment card industry (PCI) data security standards (DSS)
  - International Organization for Standardization (ISO)
  - Health Insurance Portability and Accountability Act (HIPAA)
- control access using VPC security groups
- control access using IAM to set up users, groups, and roles and assign access permissions.

#### Encryption

- data imported into FSx for Lustre is encrypted

### Snapshots

- create manual/ automatic daily backups of persistent file systems that are not linked to S3

## considerations

## integrations

- generally appears as a native file system to compute services
- AWS consideres horizontal vs vertical integrations and use cases
  - horizontal: apply across various industries and market segments
    - machine learning: sagemaker, s3
    - high perofrmance computing: eks, ec2, batch,
  - vertical: apply within an industry or market segment
    - life science, finance, industrial design, media special effects/rendering,

### s3

- process datasets When linked to an S3 bucket,
- transparently presents S3 objects as files and allows you to write changed data back to Amazon S3.

### cloudwatch

### cloudtrail

### eks

### security groups
