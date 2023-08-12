# Volume Gateway

- Hybrid cloud block storage with local caching using the iSCSI protocol.
- makes copies of your local block volumes and stores them in a service-managed Amazon S3 bucket.

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/volume/?nc=sn&loc=2&dn=4)

## best practices

### anti patterns

## features

- presents cloud-backed iSCSI block storage volumes to your on-premises applications.
- stores and manages on-premises data in S3 and operates in either cache mode or stored mode.
- take point-in-time copies of your volumes using AWS Backup

### pricing

## basics

### Cached mode

- primary data is stored in S3, while retaining your frequently accessed data locally in the cache for low latency access

### stored mode

- primary data is stored locally and your entire dataset is available for low latency access on premises while also asynchronously getting backed up to S3

### snapshots

- volumes can be asynchronously backed up as point-in-time snapshots of your volumes and stored in the cloud as EBS snapshots.
- using the service’s native snapshot scheduler or by using the AWS Backup service
- use it for disaster recovery using EBS snapshots or cached volume clones.

## considerations

## integrations

### Backup

- supports backup and restore of both cached and stored volumes
- helps you centralize backup management, reduce your operational burden, and meet compliance requirements.
