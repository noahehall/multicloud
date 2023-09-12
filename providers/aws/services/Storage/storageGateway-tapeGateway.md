# Tape Gateway

- Back up and archive on-premises data to S3 archive tiers for long-term retention.

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/vtl/?nc=sn&loc=2&dn=3)

## best practices

### anti patterns

## features

- replace physical tapes on premises with virtual tapes in AWS without changing existing backup workflows
- supports all leading backup applications and caches virtual tapes on premises for low-latency data access
- encrypts data between the gateway and AWS for secure data transfer
- compresses data and transitions virtual tapes between S3 and S3 Glacier Flexible Retrieval, or S3 Glacier Deep Archive, to minimize storage costs.

### pricing

## basics

- a virtual tape library (VTL) to your backup application using storage open standard iSCSI protocol.
  - consists of virtual tape drives and a virtual media changer.
  - immediate or frequent access to data
    - stored in service-managed S3 buckets, and creates new virtual tapes automatically
  - long term storage
    - archive tier that sits on top of S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive

### Standard Tape Gateway

- Back up and archive on-premises data to virtual tapes on AWS using your network.

### Snowball Tape Gateway

- Migrate petabytes of data stored on physical tapes to AWS using AWS Snowball.

## considerations

## integrations
