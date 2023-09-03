# s3 glacier

- Long-term, secure, durable storage classes for data archiving at the lowest cost and milliseconds access
- retain data for months, years, or decades.
- optimized for situations where you are required to retain large amounts of data for future analysis, reference, legal or compliance requirements.

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
- for each object in ANY glacier storage class
  - adds 40 KB of chargeable overhead for metadata
    - 8 KB charged at S3 Standard rates
    - 32 KB charged at S3 Glacier or S3 Deep Archive rates
- provisioned capacity: for Flexible Retrieval & Deep Archive
  - pay a fixed, up-front, monthly fee to ensure the availability of expedited retrieval capacity
- restore request: must complete a restore request job before you can view the output.
  - After completion, you have 24 hours to download the restored object
  - use SNS topic to get notifid when the job is completed

#### Instant Retrieval

- reads: immediate
- for archiving data that is rarely access and requires millisecond retrieval
  - cost savings up 68% off standard-IA storage class
  - same latency and throughput performance of standard-IA
- objects
  - 128 KB minimum object size
  - transition data into either S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive

#### Flexible Retrieval

- reads:
  - Expedited (1–5 mins) paid
  - Standard (3–5 hours) paid
  - Bulk (5-12 hours) free
- data that does not require immediate access but needs the flexibility to retrieve large sets of data at no cost
- low cost storage for archived data that is accessed 1 or 2 times per year
  - backup, disaster recovery
- objects
  - Minimum storage duration of 90 days
  - transition data to S3 Glacier Deep Archive

#### Deep Archive

- reads: up to 12 hours
- the lowest cost storage class for long-term retention (7 or more years) and digital preservation of data access 1-2 times per year
  - for meeting regulations/laws/compliance/financial/healthcare/public/etc
- objects
  - Minimum duration requirement of 180 days of storage

## considerations

## integrations
