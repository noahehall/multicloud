# Quantum Ledger Database (QLDB)

- a fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log ‎owned by a central trusted authority.
- Amazon QLDB is not a blockchain or distributed ledger technology.

## my thoughts

## links

- [landing page](https://aws.amazon.com/qldb/?did=ap_card&trk=ap_card)
- [resources](https://aws.amazon.com/qldb/resources/)

## best practices

### anti patterns

## features

- Model state changes in a cryptographically provable manner
- tracks each and every application data change and maintains a complete and verifiable history of changes over time.
- Built-in cryptographic verification enables third-party validation of data changes.
- Build correct, event-driven systems with QLDB ACID transactions and support for real-time streaming to Amazon Kinesis.
  - supports transactions with ACID semantics, a flexible document data model, and a simple query language.
- serverless architecture that provides automatic storage and resource scaling.
  - you dont provisioning capacity or configuring read and write limits.
  - You create a ledger, define your tables, and QLDB automatically scales to support the demands of your application.

### pricing

- write IO: .7 per million requests
- read IO: .136 per million requests
- data transfer: occurs in steps: check the docs
- storage: GB/month; data you have written, in addition to history, indexes, and system-generated metadata.
  - journal storage: .03
  - indexed storage: .25
  - FYI
    - no additional charge for data transferred between Amazon QLDB and other AWS services within the same AWS Region.
    - Data transferred across AWS Regions is charged on both sides of the transfer.

## terms

## basics

- general flow
  - create a ledger, define your tables, and it automatically scales to support the demands of your application.
  - data is written to the ledger, it is first committed to the journal via a transaction
  - then the data is materialized tables, allowing for faster and more efficient queries.

### High Availability

- deployed across multiple Availability Zones with multiple copies per Availability Zone
- redundancy within the Region and ensures full recoverability from Availability Zone failures
- A write is acknowledged only after being written to multiple Availability Zones, and hence, Amazon QLDB is strongly durable.

### ledger

- consists of a set of tables and a journal that maintains the complete, immutable history of changes to the tables.

#### journal

- immutable transactional log
- provides immutability and verifiability by storing a log of each transaction in a sequenced way, using cryptographic techniques
- Although the data in the table can change, the journal itself is append-only
- consists of a sequence of blocks, each cryptographically chained to the previous block so that changes can be verified
- Blocks, in turn, contain the actual changes that were made to the tables, indexed for efficient retrieval
- journal state: analogous to database Voews
  - user view: the default query view; Shows the latest non-deleted revision of your application-defined data only
  - committed view: includes everything from the user view plus access to system-generated metadata
    - e.g. to get information on the hash and the block address for the returned data
  - history view: includes everything from the commited view; generated by running a history function in the query;
    - returns a combination of all recorded revisions of the document based on the committed view
    - you'll have access to the history as well as the document’s metadata.

#### digest

- a secure output file of your data’s change history
- acts as proof of your data’s change history, allowing you to look back and validate the integrity of your data changes.
- a cryptographic hash function--in this case SHA-256 to chain all transactions together and create a concise summary of the data’s change history

#### tables

- exist within a ledger and contain a collection of document revisions. This can include a record of the deletion of a document.

#### Documents

- exist within tables and must be in Amazon Ion form
- immutability: if the documents are updated or deleted, the record of the change is itself unchangeable and cannot be altered.

#### Ion

- a superset of JSON that adds additional data types, type annotations, and comments
- [requires PartiQL for queries](./partiql.md)

### security

- authnz: IAM

## considerations

## integrations

### IAM

### CloudWatch