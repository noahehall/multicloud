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

- reads: immediate
- for archiving data that is rarely access and requires millisecond retrieval
  - cost savings up 68% off standard-IA storage class
  - same latency and throughput performance of standard-IA

#### Flexible Retrieval

- reads: 1-5 minutes (paid) to 5-12 hours (free)
- data that does not require immediate access but needs the flexibility to retrieve large sets of data at no cost
- low cost storage for archived data that is accessed 1 or 2 times per year
  - backup, disaster recovery

#### Deep Archive

- reads: up to 12 hours
- the lowest cost storage class for long-term retention (7 or more years) and digital preservation of data access 1-2 times per year
  - for meeting regulations/laws/compliance/financial/healthcare/public/etc

## considerations

## integrations
