# databases

- [also check the databases dir](./databases)

## links

- [cap theorem](https://en.wikipedia.org/wiki/CAP_theorem)
- [database transactions](https://en.wikipedia.org/wiki/Database_transaction)

## best practices

- use purpose built dbs instead of general purpose
  - refrain from having a single, shared general purpose db
  - purpose built DBS excel in as peicfic domain with unmatched performance relative to general purpose dbs
  - deploy mulitple DB engines for specific needs and run analytics across a data warehouse
  - match the data store to the business need and the type of transactions it needs to support
- model your data stores into transactional vs query needs using concepts from CQRS to design for the type of work the db needs to do
- using multiple dbs requires
  - managing distributed transactions/partial execution failures with business logic/step functinos
  - source of truth is scatterred across data stores and must be shared with other domains
  - embrace eventual consistency
  - choose the right ETL pattern

## basics

- OLTP workloads: online transactional processing
  - focus on recording Update, Insertion, and Deletion data transactions
- OLAP workloads: online analytical processing focusing on read operations, e.g. a data warehouse
  - store historical data that has been input by OLTP
  - can extract information from a large database and analyze it for decision-making
- command query respnsibility segregation: aka polyglot persistence
  - having a single big db instance thats queried by analytics services to provide `views` into the data for specific microservices/consumers
- consistency patterns: all about ensuring previous writes are reflected in future reads
  - strongly consistent: i.e. read after write; all writes are always reflected
  - eventually consistent: previous writes may NOT be reflected
  - always design around eventually consistency
    - strongly consistency means you received the latest, but subsequent writes it may be stale whenever based on previous reads
- sharding: enables horizontal scaling to serve more requests
  - distribute your data volume across multiple db instances for storage flexibility
  - the entire dataset is partitioned amongst multiple db instances
  - items stored in a particular partition is based on a hash of a partition key
- no/newsql
  - all about minimizing CPU time relative to data storage and i/o
  - data storage is now much cheaper than cpu time, hence the popularity of nosql dbs
  - does have higher i/o costs as generally you have to retrieve duplicated data at each trip
  - provides greater flexibilty in structuring your data, since your not concerned with normalization
  - OLTP with known request patterns
    - live/interactive applications
    - hot/active data
    - smaller documents
    - known query patterns

## purpose built databases

- data stores that solve specific data needs
  - READ heavy
  - WRITE heavy
  - scalability, faol-tolerence, availability
  - ACID, CAP-thereom
  - data structure

## data sources

- structured: stored as a series of data values in related tables for complex queries and analysis
- unstructured: stored as files (e.g. media, video, documents, etc), thus querying requires specialized tools to catalog and query
- semistructured: stored in json/xml files that are loaded at runtime; the structure is highly flexible enabling some querying and analysis but not at the level of structured data

## relational

- structured, normalized schemas
- all about minimizing data storage via normalization
- this strict requirement on deduplication causes some queries to be cumbersome/cpu intensive
- either OLTP or OLAP, anything with unknown query patterns
  - only normalized data can efficiently respond to unknown queries
- Normalized relational or dimensional data warehouse
- Optimized for storage
- best scaled vertically
- referential integrity, ACID transactions, schema-on-write

## Non Relational

- NoSQL: un/semistructured, lacking the ACID mechanisms of relational dbs
- NewSQL: un/semistructured, gaining traction with implementing ACID mechanisms similar to that of relational DBs
- Denormalized document, wide column, or key-value
- Optimized for compute
- best scaled horizontally
- OLTP web/mobile apps

### document

- store semi/unstructured data as some type of file

### graph

- for traversing multi-layered relationships and highly connected datasets purposely built for semi/un/structured data
- In a graph data model, relationships are first-class citizens, and the data is modeled as nodes (vertices) and links (edges)
- Nodes: are usually a person, place, or thing
- links: how nodes are all connected
- graph workloads: tracking relationships between entities.
  - complex domain model: large data set with many different entities
  - variable schema: similar entities may have different properties
  - variable structure: highly connected entities in many different ways
  - connected queries: navigate connected structures taking into account strength, weight, or quality of relationships

#### Identity Graphs

- a single unified view of customers and prospects by linking multiple identifiers such as cookies, device identifiers, IP addresses, email IDs, and internal enterprise IDs to a known person or anonymous profile using privacy-compliant methods
- captures customer behavior and preferences across devices and marketing channels.
- acts as a central hub and drives targeted advertising, personalization of customer experiences, and measurement of marketing effectiveness.

#### knowledge graphs

- a means of structuring and organizing information for improved access and understanding
- build systems that can recommend people to projects, connect related projects, or centralize access to avoid duplicate efforts.
- build systems that can recommend people to projects, connect related projects, or centralize access to avoid duplicate efforts.
- build context-aware systems that can derive answers based on queries and a vast knowledge base
- use machine learning services with knowledge graphs for better decision making and knowledge discovery.

#### Fraud Detection

- stores the relationships between the transactions, actors, and other relevant information to help customers find common patterns in the data and build applications that can detect fraudulent activities.
- people can collude to commit fraudulent transactions, creating fraud rings.
  - can have hundreds of members, which makes it challenging to find the bad actors and detect fraud.

#### Social Networking

- store human interactions between people, track skills, and roles at organizations.
- understand social interactions, likes, and preferences
- find beneficial connections across companies, fill occupational needs that may not be obvious from job title alone, and understand how information spreads through a population

#### Network/IT operations

- store resources, configurations, and access patterns across the technology landscape
- Storing this topology in a graph gives better insight to the critical resources in the production environment that are essential to protect and better understand whether the changes observed daily in an environment are critical, expected, or anomalous.

#### Recommendation Engines

- provide predictive capability based on existing connections in a graph
- exploit the fact that similarity breeds connections.
- make recommendations based on a well-understood phenomenon called triadic closure.
  - If a connection between Bill and Sarah exists and a connection between Bill and Terry exists, there is a tendency for a connection between Terry and Sarah to form.

#### Life Sciences

- store connections ranging from relationships between biological compounds and biochemical reactions to those connecting multiple datasets.
- used for applications such as data integration, management of research publications, drug discovery, precision medicine, and cancer research.

### key value

- optimized to store and retrieve unstructured non-relational data in key-value pairs in large volumes and in milliseconds
- high throughput, low latency reads/writes, endless scale

### in memory

- used for read-heavy and compute intensive applications that require low latency access to semi/structured data
- performance is improved because data is retrieved from in-memory data stores instead of waiting on databases and disk I/O.
- cache types
  - built in:
  - application
  - centralized: stores data externally from the database in a remote non-relational key-value database
- cache strategies
  - lazy: reactive. Data is put into the cache the first time it is requested
  - write through: proactive. Data is put into the cache at the same time it is put into the database.

### Vector

### time series

- collect, store and process data by sequences by time

### ledger

- complete, immutable and verifiable history of data changes

### data warehouses

- designed and used as repositories for analytical data
- store and maintain aggregate values generated from other databases.
- data is stored in columns instead of rows
  - and is indexed in a way that matches the way analytical queries are written.

### data lakes

- a centralized repository that allows you to migrate, store, and manage all structured and unstructured data at an unlimited scale.
- Once the data is centralized, you can extract value and gain insights from your data through analytics and machine learning.
- data catalog: provides a query-able interface of all assets stored
- primary goals: data cataloguing and processing
  - keeping track of all of the raw assets
  - keeping track of all of the new data assets and versions created by data transformation, data processing, and analytics.
- use cases
  - systems that generate so much disparate data, the primary goal is to store all of it for later use and analysis, without having any immediate goals
  - makes the data and the analytics tools available to more of your users, across more lines of business enabling them to get the business insights they need, whenever they need them.

## migration

- homogenous migrations: migrating from/to the same db engine
- heterogeneous migrations: migration from/to different db engines
- back-fill: porting data from one place to another, often used in db migrations to port data between two timestamps
- use cases
  - modernization: legacy dbs to modern db engines
  - platform switching
  - replication: frequenlty copying of data to share with other users or environments

### migration planning

- not all steps are applicable to each migration

#### envisioning and assessment

- understand the scope of work required based on your database schema, data volumes, data types, resources, and stakeholders.
  - assess your current environment, evaluate any known risks, and create a business case for the migration.
- determine availability of integrated tools that support the project plan and automate the migration as much as possible.

#### db schema conversion

- convert your database objects from the source engine to the target engine.
- includes converting your tables, indexes, constraints, foreign keys, triggers, and stored procedures.
  - but NOT migrating the actual data records in your database.
- for features not supported by the target db
  - you will need to recode the objects using design patterns based on the target engine’s capabilities.

#### application conversion and remediation

- process of porting application code, written in languages such as Java or C, to your new target database.
- arguably the most complex aspect of a migration process.
  - applications running against your database maybe developed long ago by resources who are no longer available.
  - These applications are often mission critical and may be difficult to rewrite without extensive research and testing.

#### scripts conversion

- looks at batch scripts used for:
  - extract, transform, and load (ETL) processes;
  - database maintenance;
  - disaster recovery;
  - etc
- scripts may not directly relate to the applications using the database, but require analysis to ensure that they work on the new database engine.

#### integration with third party services

- when you have to support databases for third-party applications, or have third-party applications access their database.
- applications like third-party business intelligence and ETL tools
- identify these third-party applications, and validate that they continue to work post-migration
- process may involve upgrading the third-party tools, or changing adapters or APIs to connect to your new databases.
- Other third-party applications might be tightly coupled to a third-party database.
  - consider whether you will maintain a legacy database for these applications, or whether you want to migrate from them.

#### data migration

- the process of moving data records from the source to the target.
- challenging if you are dealing with large data volumes and have to keep the source and target systems in sync until you can “cut over” your applications to the target system.
- if you’re changing the type of your target database, then you will have to translate most (if not all) data values to conform to the target system requirements.

#### functional system tests

- confirm that the migration went as planned
- ensure that all applications interacting with the database perform as before, from a functional perspective.
- typically involves business stakeholders and analysts who understand user-facing applications and can drive test cases that exercise system boundaries.

#### performance testing

- confirm that the migration is working correctly
- applications have specified performance criteria to meet as part of functional testing
- sometimes occurs in parallel with functional testing. This activity involves both business stakeholders and technical personnel.
- When a performance issue is discovered, each system level is checked for bottlenecks,
  - the user-facing application,
  - the SQL statements prepared by the application,
  - the database engine
  - associated storage layer

#### integration and deployment

- process of cutting over to your new database system
- typically involves a series of steps detailing how applications will be cut over to the new database system.
- Depending on business needs, the deployment may require:
  - minimized downtime.
  - specified time window
  - be performed in phases, where individual applications are cut over one by one.
- it is important to plan for what to do in case you need to roll back changes
  - test this plan in your preproduction environment as well, so the team is ready for any issues that occur during the rollout.

#### training and knowledge transfer

- Training is a critical aspect in deploying any new system.
- ensure that db/system/application changes are documented, and share that knowledge with team members who support and maintain the application.
- the move to new technologies also changes the relative status of people within the team.
  - introducing undercurrents of organizational politics into the process.
- develop features to manage the new environment, such as monitoring and paging.
  - Many of the features you used previously may not be available for your new database.

#### documentation and version control

- documentation is one of the most important tasks prior to putting a system into production.
- document all changes that have been made to the system, and how the new system operates.
- this is a good opportunity to automate all manual steps.
  - consider treating all of these artifacts as code.

#### post-production support

- Once the migrated application is running, you will need at least some support
- automate backups and other support tasks, but it is a good idea to plan for support your application might need
- Ensure that automated tasks are occasionally checked, and that you have personnel for tasks that are not automated.

# copypasta from somewhere else

- database technology predates the web, since the 1960s
- SQL databases
  - are relational, storing data in one/more tables that related to each other in formally prescribed ways
  - DDL: data definition language
    - any statement using CREATE, DROP or MODIFY to create, drop and modify table structures
  - DML: data manipulation language
    - any statement using SELECT, INSERT, UPDATE, and DELETE for CRUDing records
- NoSQL databases
  - sacriface the strict data integrity requirements of SQL databases to achieve greater scalability
  - often schemaless, allowing you to add fields to new records with having to upgarde any data structures
- distrubed caches
  - in-memory databases, that load data from disk and stores it in cache
  - caching refers to the process of storing a copy of data in an easily retrievable form to speed up responding to requests for that data

### data fabric

### data mesh

- enables collection, integration and analysis of data from disparate systems concurrently in a single location

### data lake

- allows storing yuuuge amounts of raw, structured, and/or unstructured data in a single repository enabling comprehensive analysis from a single location
  - i.e. you push any and everything into a data lake, whether or not the data has a purpose

### data warehouse

- allows storing yuuuge amounts of structured, filtered data that has already been processed for a specific purpose (like data already in use by app/biz)

  - i.e. you push filtered data into a warehouse, for later analysis

### Databases

#### Relational

- relational, structured
- best for online analytical processing

#### NewSql

#### nosql

- hierarchical, unstructured
- best for online transaction processing at scale
- scales out
- main distinctions are data model driven: e.g. rdbms vs nosql vs wide-column etc
- secondary distinctions is according to the cap thereom: usually a choice between C and A, as all are susceptible to P (failures)
  - consistency: will every read receive the most recent write
  - availability: will ever request receive a non-error response (but doesnt have to be the most recent write)
  - partition tolerance: will the system operate in the face of network failures/msg loss

##### key/val

- data model
  - key value pairs: each key has only one value
  - fast queries, no need for a query language

##### document store

- data model
  - data stored in documents of tagged elements (like a row in rdbms)

##### column (oriented)

- long list
- data model
- characteristics
  - wheres SQL stores record by record (row by row), column oriented dbs store column by column
  - improves storage and retrieval performance

##### wide column store

- long list
- data model
  - store data in columns
  - related columns are grouped into tables (column families)
- characteristics
  - supports large numbers of dynamic columns:
  - aka 2 dimensional key value stores

##### graph

- long list
- data model
  - use nodes & edges to store data

##### multi-model

- go for the native multi models, and not the ones enhanced with extensions/plugins/etc (timescale is dope tho)
