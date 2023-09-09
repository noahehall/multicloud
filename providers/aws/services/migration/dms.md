# Database Migration Service (DMS)

- data migration
- assess, convert and migrate databse and analytic workloads to AWS

## links

- [data sources](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.html)
- [landing page](https://aws.amazon.com/dms/?did=ap_card&trk=ap_card)
- [resources & playbooks](https://docs.aws.amazon.com/dms/)
- [sct: extension pack](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_ExtensionPack.html)
- [sct: landing page](https://aws.amazon.com/dms/schema-conversion-tool/?nc=sn&loc=2&refid=ap_card)
- [sct: user guide](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html)

## best practices

### anti patterns

## features

- migrate non/aws databases to other aws databases across regions and AZs
- maintain high availability and minimal downtime during the migration with multi-az and ongoing data replication and monitoring
- supports hetero/homogeneous db migrations
- Create redundancies of business-critical databases and data stores to minimize downtime and protect against any data loss.
- Build data lakes and perform real-time processing on change data from your data stores.

### pricing

- on demand: pay for
  - replication instances
  - any additional log storage
  - by the hour used
- serverless: pay for
  - capacity that is use

## terms

## basics

- general workflow
  - identify source and target data stores
  - setup configure endpoint connections
  - manually create an EC2 instance, and DMS will configure it with replication software
  - DMS manages and runs replication tasks, providing you with updates
  - DMS/you create the target tables and begins the migration
    - Loads the tables with data without any foreign keys or constraints
    - select which schemas or tables to include in the migration, filter out unwanted data records, and transform names to conform to your particular naming conventions
  - Data validation is an option that you can choose to add to your replication task
    - For relational migrations: DMS can validate the migrated data
    - tracks the progress of the migration and incrementally validates new data as it is written to the target.
- endpoints: databases that reside within AWS
  - The only requirement to use AWS DMS is that one of the endpoints is on an AWS service

### full-load migration

- perform a one-time copy of the source data to the target.
- existing data from the source is moved to the target,
- AWS DMS loads data from tables on the source data store to tables on the target data store.

### change data capture (CDC)

- data replication: keeps your target in sync with a transactionally active source

### Integration and deployment

- integration: Use AWS DMS to perform a full-load migration, and then set up ongoing replication for change data capture
- deployment: verify that data is flowing correctly,
  - change the application to talk to the new database.
  - stop stop the replication task

### Monitoring migrations

- monitor the migration process and check the health of your migration resources via the AWS DMS console.
- A migration task runs on a replication instance (EC2) configured with the DMS application

## AWS Schema Conversion Tool (SCT)

- object migration
- for heterogenous migrations to translate the source schema to the target schema
- Identifies the issues, limitations, and actions for the schema conversion
  - catalogues the physical and logical components of the existing system
- provide a detailed report on each engine, and you can choose the best target for your particular use case.
  - shows you what AWS SCT can convert automatically, which objects need manual remediation, and which objects require significant remediation.
  - include which features are not supported by your target database, and recommendations on how to translate
- converts source schema & code to Generatee the target schema scripts, including
  - foreign keys and constraints
  - convert tables and indexes, stored procedures and functions, database packages, and views and triggers
  - anything that cant be converted is marked for manual conversion
- use AWS SCT to extract SQL statements that are embedded in your application code.
  - will track all the places where SQL is present, convert the SQL to work with the target database, and rebuild your application program with the converted code.

### Extension Pack

- add-on module that emulates functions present in the source database that are required when converting objects to the target database.
- Before you can install the AWS SCT Extension Pack, you need to convert your database schema.

## Migration Playbooks

- currently offers five migration playbooks and will release more over time.
- a series of guides focused on best practices for creating successful blueprints for heterogeneous database migration.
- its all about determining the appropriate pattern when the target db doesnt support some feature in the source db

## considerations

## integrations
