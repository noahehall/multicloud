# databases

## best practices

- sharding use cases: all about scaling horizontally
  - distribute your data volume across multiple db instances for storage flexibility
  - serve increased request rates
- consistency patterns: all about ensuring previous writes are reflected in future reads
  - strongly consistent: i.e. read after write; all writes are always reflected
  - eventually consistent: previous writes may NOT be reflected
  - always design around eventually consistency
    - strongly consistency means you received the latest, but subsequent writes it may be stale whenever based on previous reads
- use purpose built dbs instead of general purpose
  - purpose built DBS excel in as peicfic domain with unmatched performance relative to general purpose dbs
  - deploy mulitple DB engines for specific needs and run analytics across a data warehouse

## basics

- OLTP workloads: online transactional processing
  - focus on recording Update, Insertion, and Deletion data transactions
- OLAP workloads: online analytical processing focusing on read operations, e.g. a data warehouse
  - store historical data that has been input by OLTP
  - can extract information from a large database and analyze it for decision-making
- command query respnsibility segregation: aka polyglot persistence
  - having a single big db instance thats queried by analytics services to provide `views` into the data for specific microservices/consumers
- sharding: enables horizontal scaling
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
- NewSQL: un/semistructured, gaining traction on the ACID mechanisms of relational DBs
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
-

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

## migration planning

- homogenous migrations: migrating from/to the same db engine
- heterogeneous migrations: migration from/to different db engines
- back-fill: porting data from one place to another, often used in db migrations to port data between two timestamps
