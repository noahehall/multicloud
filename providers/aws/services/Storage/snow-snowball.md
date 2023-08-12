# Snowball Edge

- edge computing, data migration, and rugged edge storage device
- move offline/remote data to the cloud

## my thoughts

## links

- [landing page](https://aws.amazon.com/snowball/)

## best practices

### anti patterns

## features

- migrate terabytes of data to the cloud without limits in storage capacity or compute power.
  - Move databases, backups, archives, healthcare records, analytics datasets, IoT sensor data and media content to the cloud
- run compute workloads with little or no connectivity.
- ruggedized chassis, integrated logistics, and tamper-evident box
  - 8.5 G impact-resistant external case that also provides rain and dust resistance.
- work with or without the internet, do not require a dedicated IT operator, and are designed to be used in remote environments
- use cases
  - Data collection
  - Machine learning and processing
  - Storage in environments with intermittent connectivity or in remote disconnected locations

### pricing

- pay for use of the device and for data transfer out of AWS.
  - ondemand
  - commited (upfront):

## basics

- cluster multiple Snowball Edge devices to create a local storage tier with increased durability for your on-premises applications.
  - not available for data transfer only jobs

### Storage Optimized

- Storage Optimized without compute
  - 80 TB of S3 compatible storage without available vCPUs or Amazon EBS storage.
  - use cases
    - data transfer-only jobs at a reduced cost.
- Storage Optimized with compute
  - highest storage capacity
    - 24 vCPUs of compute capacity
    - 80 terabytes (TB) of usable block or S3-compatible object storage.
  - use cases
    - local storage
    - large-scale data transfers.

### Compute Optimized

- more available vCPUs with a lower storage capacity.
  - 52 vCPUs
  - 42 TB of usable block or s3-compatible object storage
  - optional GPU for high performance computing
    - NVIDIA Tesla V100 GPU and Amazon EC2 instances

### Storage

#### Object Storage

- s3-compatible, endpoint accessible via SDK or cli
- NFS endpoint available for simple file transfers if s3 adapter isnt feasible or interfacing with existing NFS workloads
- file system metadata is preserved until the objects are converted to objects and uploaded to s3

#### Block storage

- available for both Storage and Compute optimized edge devices
- attach block storage to ec2 instances using a subset of the EBS api
  - enables development of applications in EC2 that run in disconnected/remote locations
- EBS volume types
  - performance optimized
  - capacity optimized

### Security

#### Encryption

- encrypted with 256-bit encryption keys that KMS manages.
- at rest
- in transit
  - encryption is performed on the device itself, helping to enable a higher data throughput rate and shorter data transfer times.s

## considerations

## integrations
