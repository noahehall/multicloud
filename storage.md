# Storage

- Network File System: FNS; generally linux based systems
- Server Message Block: SMB; generally windows
- SAN: storage area networks
- IOPS: input/output operations per second
- R/W patterns
  - Worm: write once, read many: for data with heavy reads

## links

### tools

- [Flexible IO: load tester](https://github.com/axboe/fio)
- [io meter: load tester](http://www.iometer.org/doc/downloads.html)
- [sysbench: load tester](https://github.com/akopytov/sysbench)
- [oracle orion: load tester](https://docs.oracle.com/cd/E18283_01/server.112/e16638/iodesign.htm#BABFCFBC)

## Consideration

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
