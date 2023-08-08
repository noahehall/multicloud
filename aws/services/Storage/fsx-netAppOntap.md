# FSx NetApp OnTAP

- designed to provide both NetApp block and file storage. The access protocols are iSCSI for block storage, NFS and SMB for file storage.

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

### Capacity pool storage

- elastic storage that’s cost-optimized for infrequently-accessed data.
- automatically transitions data from SSD storage to capacity pool storage based on access patterns

## considerations

## integrations
