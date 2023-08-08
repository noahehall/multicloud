# s3 glacier

- Long-term, secure, durable storage classes for data archiving at the lowest cost and milliseconds access

## my thoughts

## links

- [landing page](https://aws.amazon.com/s3/storage-classes/glacier/)

## best practices

### anti patterns

## features

- purpose-built for data archiving

### pricing

## basics

### Storage Classes

- optimized for different access patterns and storage duration

#### Instant Retrieval

- for archiving data that is rarely access and requires millisecond retrieval
  - cost savings up 68% off standard-IA storage class
  - same latency and throughput performance of standard-IA

#### Flexible Retrieval

- data that does not require immediate access but needs the flexibility to retrieve large sets of data at no cost
- retrieval in minutes or free bulk retrievals in 5-12 hours
  - data can be retrieved between 1-5 minutes for a cost
- low cost storage for archived data that is accessed 1 or 2 times per year
  - backup, disaster recovery

#### Deep Archive

- the lowest cost storage class for long-term retention (7 or more years) and digital preservation of data access 1-2 times per year
  - for meeting regulations/laws/compliance/financial/healthcare/public/etc
- data can be retrieved in about 12 hours

## considerations

## integrations
