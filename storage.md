# Storage

- theres some
  - copypasta at the bottom
  - duplication in this, databases, and distributedSystems files

## links

- [RAID](https://en.wikipedia.org/wiki/Standard_RAID_levels)

### tools

- [Flexible IO: load tester](https://github.com/axboe/fio)
- [io meter: load tester](http://www.iometer.org/doc/downloads.html)
- [sysbench: load tester](https://github.com/akopytov/sysbench)
- [oracle orion: load tester](https://docs.oracle.com/cd/E18283_01/server.112/e16638/iodesign.htm#BABFCFBC)
- [iostat](https://www.redhat.com/sysadmin/io-reporting-linux)

## basics

- cap theorem: any distributed data store can only provide 2 of three guarantees: consistency, availability, and partition tolerance; since every DB is susciptible to partition failure, its really a choice between consistency and availability
- consistency: every read receives the most recent write/error
- availability: every request receives a (non-error) response, without the guarantee that it contains the most recent write
  - can be overcome via active replication: in the event of failure just switch to the redundant system
- partition tolerance: the system continues to operate despite an arbitrary number of messages being dropped/delayed by the network between nodes
- network partition failure: forces you to either cancel the operation & decrease availabilty but ensure consistency, or proceed with the operation and thus provide availability but risk consistency
- garbage collection
- compaction
- node: generally a unit of storage, e.g. a single db instance including all the software running on the hardware
- cluster: a group of nodes that work together; e.g. a 3 node cluster is the minimum for high availability
- replication: the process of replicating data across nodes ina cluster
- replication factor: The total number of replica Nodes across a given Cluster; the number of copies of a set of data, e.g. RF of 1, means theres 1 copy, RF 2 means there 2 identical copies, etc, generally you want 3
- consistency level: how many nodes must validate a READ/WRITE before the request is considered successful
  - e.g. at least 2 nodes must acknowledge an operation/query/whatever for it to be considered 200
- big data: generally the dataset is so huge it cant be contained in a single node, thus a cluster of nodes are required

### protocols

- Network File System: FNS; generally linux
- Server Message Block: SMB; generally windows
- SAN: storage area networks

### R/W patterns

- determine whether your workload is random or sequential via monitoring graphs
  - The more sequential your workload is, the more bandwidth it will consume and the larger your average read/write sizes will be as reported in the graphs.
- HDD volumes: the seek head needs to move across the various areas of the spinning disk to access the blocks
  - If the blocks are randomly scattered, then the head must move back and forth to the different blocks, creating latency.
- SSD volumes: sequential write operations comprise a number of simultaneous individual write operations to distinct data cells.

#### Worm

- write once, read many: for data with heavy reads

#### random:

- data is read or written from data scattered around the storage medium
- If there's an open block, a small I/O operation can write to it

#### sequential:

- data is read or written from long contiguous sections.

## Considerations

- characteristics that will lead you toward the best storage solution
  - Type of access method (block, file, or object)
    - storage access protocols: applications are often developed based on specific operating systems
    - How much storage capacity is required for each type
  - Patterns of access (random or sequential)
  - Required throughput
    - How should you configure your volumes?
    - What are the recommended block sizes? What are the cache sizes?
    - Frequency of access (online, offline, archival)
    - Frequency of update (WORM, dynamic)
  - Availability and durability constraints
    - Is there temporary data that does not need to persist?
    - Does data need to be shared? which services need to share the data? How does it need to be shared?
- optimization: identify constraints & service options for improvement
  - understand the performance profile for each of your workloads
  - performance analysis to measure input/output operations per second (IOPS), throughput, and other variables
  - metrics improvement strategies as part of a data-driven approach, benchmarking/load testing
    - Define your storage performance requirements
    - Identify your workload’s most important storage performance metrics
- Q/A
  - storage-data-application workflow
    - New/Existing
    - requirements: interopability with other systems, configuration, retention, compliance, business, access patterns
    - Use case: high performance computing, databases, static content, backup/retrieval heavy, read vs write heavy, authnz/data sharing
  - Storage
    - location: remote, edge, hybrid, regional
    - type: block/file/object/combination
    - performance: iops/throughput/latency/transactions
  - Data
    - access protocols: rest/protobuff/nfs/ftp/smb
    - transfer performance: over the wire/physical/between services/redundancy
    - protection: backups, retention, compliance, longterm archival, point-in-time snapshots
- workload considerations
  - IOPS-intensive (SSD) or throughput-intensive (HDD)
  - latency sensitivity:
    - high performance IOPS: very low and sub-millisecond to single-digit millisecond latency is needed
    - midgrade IOPS: single-digit to low two-digit latency is tolerable
    - not latency sensitive: go with the cheapest option available

### Onpremise capacity calculations

- raw: what you pay for and used calculate the operating costs and data center requirements.
  - The net usable capacity will vary by manufacturer and by individual system.
- formatted: raw capacity - hardware failure protection overhead, drive formatting, and operating system overhead.
  - Hardware failure protection overhead: aka hardware or software redundant array of independent disks (RAID)
    - protects the data if hardware or a drive fails by creating checksum protection for the data
    - Depending on the protection level, this can amount 15%–50% overhead
  - Formatting and operating system overhead: The operating system is then added to the system, which further reduces the available capacity 1%–5%
- allocated: formatted capacity - data protection services, such as snapshots, and add space for performance overhead
  - Snapshots can consume more space than your actual data
  - systems require additional space for operation overhead, especially for write operations.
- actual: business requirements + allocated:

### RAID

- redundant array of independent disks
- whenever an application or workload needs more performance from the volumes than any single volume can provide.
- use cases
  - can increase your volume size
  - improve performance,
  - provide additional fault tolerance for your data.
- There are seven types/levels of RAID 0 -> 6:
  - Each having a different disk configuration, data layout and parity pattern, and each type addresses different performance needs
  - Both RAID 3 and RAID 4 were replaced by RAID 5.

#### raid 0 (stripe set or striped volume)

- splits ("stripes") data evenly across two or more disks, without parity information, redundancy, or fault tolerance.
- the failure of one drive will cause the entire array to fail; as a result of having data striped across all disks, the failure will result in total data loss
- use cases
  - increase performance,
  - create a large logical volume out of two or more physical disks

#### raid 1 (mirror)

- consists of an exact copy (or mirror) of a set of data on two or more disks
- offers no parity, striping, or spanning of disk space across multiple disks, since the data is mirrored on all disks belonging to the array, and the array can only be as big as the smallest member disk
- use cases
  - read performance or reliability is more important than write performance or the resulting data storage capacity.

#### raid 2

- rarely used in practice
- stripes data at the bit (rather than block) level, and uses a Hamming code for error correction

#### raid 3

- rarely used in practice
- consists of byte-level striping with a dedicated parity disk.

#### raid 4

- consists of block-level striping with a dedicated parity disk
- provides good performance of random reads, while the performance of random writes is low due to the need to write all parity data to a single disk

#### raid 5

- consists of block-level striping with distributed parity. Unlike in RAID 4, parity information is distributed among the drives
- requires that all drives but one be present to operate. Upon failure of a single drive, subsequent reads can be calculated from the distributed parity such that no data is lost

#### raid 6

- extends RAID 5 by adding another parity block; thus, it uses block-level striping with two parity blocks distributed across all member disks.[28]
- does not have a performance penalty for read operations,
- does have a performance penalty on write operations because of the overhead associated with parity calculations.

## storage types

### block storage

- raw storage in which the hardware storage device or drive is a disk or volume that is formatted and attached to the compute system for use
- logical block addressing: abstraction layer that allows the operating system (OS) to read and write data in logical blocks without knowing much about the underlying hardware.
- splits data into chunks (aka blocks; each with distinct addresses) and stores them on disk, subject to fragmentation over time
- its more efficient when changing a piece of the data, as only the chunk needs to be updated
- R/W pattern: WORM
- examples: HDDs, SSDs, NVMe, SAN systems
- use cases: transactional workloads, containers, virtual machines, i/o intensive apps, operating systems, databases, big data analytics engines
  - used by the operating system or an application that has the capabilities to manage block storage directly
- architecture
  - the block storage: physically/logically attached to the compute system
  - the compute system:
  - the OS/application: runs on the the compute system, manages the block storage
    - recognizes the block storage as available and formats or makes the block storage avaible for use
      - an app running on the compute system can act as the managing entity rather than the OS

#### block data components

- block size: select the block size that best meets your use case
  - Block size flexibility is a fundamental differentiator for block storage
    - certain workloads benefit from a smaller or larger block size,
    - file systems support non-default block sizes that you can specify when formatting the disk
  - The industry default size for logical data blocks is currently 4,096 bytes, or 4 kibibytes (KiB).
- metadata management: information that the operating system and users need to identify and track the data.
  - e.g. resource type, permissions, and the time and way it was created.
- read/write activity: controls in place to manage access to the data.
  - uses metadata permissions to control who can access, modify, or delete the data.
  - can also reference external sources such as Microsoft Active Directory or Lightweight Directory Access Protocol (LDAP) to determine these permissions.
  - manages the caching or read activity
  - determines how writes are cached and staged before writing the blocks and which blocks to write to.
- locking control: manages data integrity when data is being modified or deleted
  - file-level locking: applying a lock on the entire data file
  - block-level locking: specific portions or blocks being modified.
- volumes: a logical storage construct that can be created from a single drive or using multiple drives

### file storage

- built on top of block storage, typically serving as a file share or file server.
  - created using an operating system that formats and manages the reading and writing of data to the block storage devices
- treats data as atomic units (e.g. a file) but also organized in a tree structure, like your filesystem
  - ideal when you require centralized access that must be easily shared and managed by multiple host computers
  - if changing a piece of data, you need to replace the entire file
- use cases: web servers, analytics, media, file systems
- storage protocols: enable users to use the servers' resources or share, open, and edit files.
  - network protocols: enable users to communicate with remote computers and servers.

#### Server Message Block: Storage Protocol

- network protocol

#### Network File System: Storage Protocol

- network protocol

### object storage

- built on top of block storage: created using an operating system that formats and manages the reading and writing of data to the block storage devices
- treats data as atomic units (e.g. a file) and stores it on disk in a flat hierarchy, not subject to fragmentation over time
  - if changing a piece of the data, you need to replace the entire object
- Unlike file storage, object storage does not differentiate between types of data
  - method of storing files in a flat address space based on attributes and metadata.
- uses cases: data archiving, backup and recovery, rich media; systems requiring file versioning, file tracking, and file retention.

# copypasta from some other file

## Storage

- cap theorem: any distributed data store can only provide 2 of three guarantees: consistency, availability, and partition tolerance; since every DB is susciptible to partition failure, its really a choice between consistency and availability
- consistency: every read receives the most recent write/error
- availability: every request receives a (non-error) response, without the guarantee that it contains the most recent write
  - can be overcome via active replication: in the event of failure just switch to the redundant system
- partition tolerance: the system continues to operate despite an arbitrary number of messages being dropped/delayed by the network between nodes
- network partition failure: forces you to either cancel the operation & decrease availabilty but ensure consistency, or proceed with the operation and thus provide availability but risk consistency
- garbage collection
- compaction
- node: generally a unit of storage, e.g. a single db instance including all the software running on the hardware
- cluster: a group of nodes that work together; e.g. a 3 node cluster is the minimum for high availability
- replication: the process of replicating data across nodes ina cluster
- replication factor: The total number of replica Nodes across a given Cluster; the number of copies of a set of data, e.g. RF of 1, means theres 1 copy, RF 2 means there 2 identical copies, etc, generally you want 3
- consistency level: how many nodes must validate a READ/WRITE before the request is considered successful
  - e.g. at least 2 nodes must acknowledge an operation/query/whatever for it to be considered 200
- big data: generally the dataset is so huge it cant be contained in a single node, thus a cluster of nodes are required

### data fabric

### data mesh

- enables collection, integration and analysis of data from disparate systems concurrently in a single location

### data lake

- allows storing yuuuge amounts of raw, structured, and/or unstructured data in a single repository enabling comprehensive analysis from a single location
  - i.e. you push any and everything into a data lake, whether or not the data has a purpose

### data warehouse

- allows storing yuuuge amounts of structured, filtered data that has already been processed for a specific purpose (like data already in use by app/biz)
  - i.e. you push filtered data into a warehouse, for later analysis
