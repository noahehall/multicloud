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
- Persistent file systems: longer-term storage and workloads. Data is replicated and file servers are replaced if they fail.
- Scratch file systems: temporary storage and shorter-term processing of data. Data is not replicated and does not persist if a file server fails.
- SSD storage: latency-sensitive workloads or workloads requiring the highest levels of IOPS/throughput.
- HDD storage: throughput-focused workloads that aren’t latency-sensitive
  - optional SSD cache: automatically places most frequently read data on SSD
    - the cache size is 20% of file system size

## considerations

## integrations
