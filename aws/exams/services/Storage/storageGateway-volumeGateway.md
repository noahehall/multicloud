# Volume Gateway

- Hybrid cloud block storage with local caching

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

## considerations

## integrations
