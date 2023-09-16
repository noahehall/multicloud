# Volume Gateway

- Hybrid cloud block storage with local caching using the iSCSI protocol.

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/volume/?nc=sn&loc=2&dn=4)
- [storage sizing](https://docs.aws.amazon.com/storagegateway/latest/vgw/ManagingLocalStorage-common.html)
- [CHAP](https://docs.aws.amazon.com/storagegateway/latest/vgw/GettingStartedConfigureChap.html)
- [isci: connecting initiators](https://docs.aws.amazon.com/storagegateway/latest/vgw/initiator-connection-common.html)
- [isci: settings](https://docs.aws.amazon.com/storagegateway/latest/vgw/initiator-connection-common.html#recommendediSCSISettings)

## best practices

- volume sizes must be steady and predictable
  - To decrease the storage capacity: create a new gateway and migrate your data to the new gateway
  - To increase storage capacity: add new disks to the gateway instead of expanding disks previously allocated.
- Volume storage is billed for only the amount of data stored on the volume, not the size of the volume you create.
  - delete older volumes and snapshots that are no longer needed.
  - Check for snapshots that are more than 30 days old and delete them to reduce storage costs.
- optimize iSCSI settings on your iSCSI initiator to achieve higher I/O performance
  - recommend choosing 256 KiB for MaxReceiveDataSegmentLength and FirstBurstLength, and 1 MiB for MaxBurstLength
- When you provision gateway disks
  - don't provision local disks for the upload buffer and cache storage that use the same underlying physical storage disk
- if a volume is used for a high-throughput application
  - consider creating a separate gateway for the high-throughput application
  - as a general rule, you should not use one gateway for all of your high-throughput applications and another gateway for all of your low-throughput applications.
  - To measure your volume throughput, use the ReadBytes and WriteBytes metrics.
- CHAP configuration is optional but AWS recommends using CHAP for secure exchange between host initiator and target.

### anti patterns

## features

- presents cloud-backed iSCSI block storage volumes to your on-premises applications.
- stores and manages on-premises data in S3 and operates in either cache mode or stored mode.
- take point-in-time copies of your volumes using AWS Backup
- use cases
  - Backups for your local applications in the cloud
  - Disaster and recovery solution plans based on EBS snapshots
  - Cached volume clones

### pricing

- vary based on the Region, storage type, and gateway host specifics.
- Storage: Fees are based on the type of storage you use with your gateway (for example, Amazon S3 and Amazon EBS) and how it is configured.
- Requests: Fees are based on data operations performed through the gateway including data ingest into AWS.
- Data transfer: Fees are based on data transferred out of the Storage Gateway service and into your Storage Gateway appliance.

## basics

- presents cloud-backed iSCSI block storage volumes to your applications to provide seamless integration between your IT environment and the Amazon Web Services (AWS) storage infrastructure for your data protection, recovery, and migration needs.
- makes copies of your local block volumes and stores them in a service-managed S3 bucket.

### Storage Volumes

- After you create and activate the gateway appliance, on premises or on AWS, you can then create gateway storage volumes.
- provisioning
  - stored volumes: storage volumes volumes are mapped to on-premises direct-attached storage (DAS) or SAN disks.
  - cache volumes: use the Console, API, or SDKs to provision storage volumes backed by Amazon S3
- mounting the volumes for your application to access: for both stored & cached volume mode
  - mounted as iSCSI devices.
- expanding volume size: Automatic resizing of a volume is not supported with Volume Gateway.
  - Create a snapshot of the volume you want to expand and then use the snapshot to create a new volume of a larger size.
  - Use the cached volume you want to expand to clone a new volume of a larger size.
- volume status: available, bootstrapping, creating, deleting, irrecoverable, pass through, restoring, restoring passthrough, upload buffer not configured,
- attachment status: attached, detached, detaching
- volume actions: creating an on-demand backup plan with AWS Backup, create an EBS snapshot, edit the snapshot schedule, configure CHAP authentication, delete a volume, connect the volume to the client (iSCSI initiator), and add optional tags.

#### Cached Volume mode

- Frequently accessed data is cached on premises for local use
  - primary data is stored in S3
  - Any data that your applications modify is moved to the upload buffer, where it is prepared for uploading asynchronously to S3.
- deployed into your on-premises or AWS Cloud environment as a VM running on VMware ESXi, KVM, Microsoft Hyper-V hypervisor, Amazon EC2, or a physical gateway hardware appliance
- range from 1 gibibyte (GiB) to 32 tebibytes (TiB) in size
  - support up to 32 volumes for a total maximum storage volume of 1,024 TiB [1 pebibyte (PiB)].
- Local disks are needed for cache storage and an upload buffer for a cached volume gateway.
- Data that is written to the Storage Gateway cache, but has not been uploaded to AWS, is referred to as dirty.
- use cases
  - Custom file shares
  - Migrating application data into Amazon S3 and transitioning to Amazon EC2
  - applications where you have a large dataset, but your application only needs low-latency access to a subset of that data at any given time.

##### cache storage

- acts as the on-premises durable store for data that is pending upload to Amazon S3 from the upload buffer
- When applications performs I/O on a volume, the gateway saves the data to the cache storage for low-latency access
- when applications requests data from a volume, the gateway first checks the cache storage for the data before downloading the data from AWS.

##### cached Reads: read-through cache

- if the data is in the cache volume: it is served locally from the cache and there is no latency. Everything happens directly inside your data center.
- If the data is not in the cache: Storage Gateway service retrieves the compressed data from Amazon S3 and sends it to the gateway appliance.

#### stored Volume mode

- primary data is stored locally
  - asynchronously backed up to S3: recover to your local data center or Amazon EC2
  - take one-time or scheduled snapshots of that volume
- only available with on-premises host platform options.
- range from 1 GiB to 16 TiB in size
  - support up to 32 volumes, with a total volume storage of 512 TiB (0.5 PiB).
- use cases
  - Block storage backups
  - Migrations or phased migrations
  - Cloud-based disaster recovery
  - applications with lots of random reads and writes, and you need full access to the data at low latency at all times.

##### Stored reads

- One hundred percent of the data on your volumes is stored locally. Therefore, any reads to this data come directly from the virtual appliance or SAN from that local disk.

#### Volume Recovery Options

- volume clone: create a new volume from any existing cached volume in the same AWS Region
  - created from the most recent recovery point of the selected volume.
  - Cloning from an existing volume is faster and more cost effective than creating an Amazon EBS snapshot
- ebs snapshot: a point-in-time copy of the volume at the time the snapshot is requested.
  - cached volume:
  - stored volume:

#### snapshots

- volumes are asynchronously backed up as Amazon EBS snapshots and stored in an S3 service bucket
  - NOT of a customer bucket: they do not showup in the s3 console
  - are ONLY accessible from the Storage Gateway and Amazon EBS management console
- use it for disaster recovery using EBS snapshots or cached volume clones.
- one-time snapshot: back up your storage volume immediately without waiting for the next scheduled snapshot.
- snapshot schedule: by default assigned a snapshot schedule of once a day
  - can be edited by specifying either the time the snapshot occurs each day or the frequency (every 1, 2, 4, 8, 12, or 24 hours), or both.
  - doesn't create a default snapshot schedule for cached volumes.
    - because your data is durably stored in Amazon S3.
  - can't remove the default snapshot schedule for stored volumes, as they require at least one snapshot schedule.

#### rate limiting bandwidth

- By default, an activated gateway has no rate limits on upload or download
- You can limit/throttle the upload throughput from the gateway to AWS or the download throughput from AWS to your gateway.
- rate limit: helps you to control the amount of network bandwidth used by your gateway.
  - The minimum rate for download is 100 Kilobits (Kib) per second
  - the minimum rate for upload is 50 Kib per second.
- rate limit schedule: assign bandwidth rate limits on a schedule

### upload buffer

- Both cached volume and storage volume gateway deployments require upload buffer storage.
- provides a staging area for the data before the gateway uploads the data to Amazon S3
- the gateway uploads this buffer data over an encrypted SSL connection to AWS.

#### cached writes: write-back cache

- Data written gets stored in the local volume cache in its native format
- Volume Gateway then compresses and encrypts the data as it moves the data from the cache into the upload buffer.
- From the upload buffer, the data is then transferred to AWS.

#### stored writes

- data writes happen directly to that disk where they are immediately acknowledged locally.
- When you create a snapshot of one of your volumes, the data will then be moved to AWS

#### Required Network Ports

- TCP 443: storage gateway VM -> AWS
- TCP 80: browser -> storage gateway VM
- UDP 53: storage gateway VM -> DNS Server
- TCP 22: Storage gateway VM -> AWS
- UDP 123: Storage gateway vm -> NTP server
- iSCI 3260: iSCI initiators -> storage gateway vm

#### data transfers

- Volume Gateway routes your block storage data through the on-premises Volume Gateway appliance to and from either storage area network (SAN) or virtual volumes.

### Challenge-Handshake Authentication Protocol (CHAP)

- provides protection against man-in-the-middle and playback attacks by periodically verifying the identity of an iSCSI initiator as authenticated to access a storage volume target
- For each volume target, you can define one or more CHAP credentials.
- To set up CHAP, you must configure it both on the Storage Gateway console and in the iSCSI initiator software that you use to connect to the target.

### iSCSI protocol

- an IP-based storage networking standard for linking data storage facilities
- SCSI: Small Computer System Interface
- provides block-level access to storage devices by carrying SCSI commands over a TCP/IP network using the default port 3260
- used to facilitate data transfers over intranets and to manage storage over long distances

#### initiators

- applications that send SCSI commands to targets
  - Connect only one iSCSI initiator to each iSCSI target.
- Storage Gateway only supports software (linux/windows) initiators.

#### Targets

- storage devices on remote servers that receive data from initiators
- For Volume Gateway, the iSCSI targets are volumes.

### security

- As a Storage Gateway user, your clients will be using a local mounted storage device, but the gateway appliance will authenticate to AWS and write your volume data to Amazon S3.

#### Encryption

- Your data is encrypted in transit and in the AWS Cloud. Data that is in the gateway cache is not encrypted.
- All data stored by Storage Gateway in Amazon S3 is encrypted server-side with either of the following:
  - Amazon S3 server-side encryption (S3-SSE) (default)
  - AWS Key Management Service (AWS KMS)
- EBS snapshots are encrypted at rest using Advanced Encryption Standard (AES-256),

## considerations

- region: Storage Gateway is only available in select regions
- volume type
  - cached: only a subset of data is needed on premise
  - stored: the full dataset is needed on premise
- gateway appliance location
  - on premise: required for stored volumes
    - virtual machine (VM) appliance: download and deploy to one of the supported host VMs
  - aws: available for cached volumes
    - deploy in the cloud as an Amazon Machine Image (AMI) in Amazon EC2
- sizing considerations
  - Determine the number of total volumes and capacity you need to support
    - cached mode: supports up to 32, 32 TiB cached volumes.
    - stored mode, supports up to 32, 16 TiB stored volumes.
  - Estimate your application and workload volume:
    - minimum requirement is to allocate at least one local disk for each of the following: - Cache storage – a minimum of 150 GiB (cached volume) - Upload buffer storage – a minimum of 150 GiB (cached and stored volume)
    - Deploy additional gateway appliances to increase overall throughput (if required).

## integrations

### Backup

- supports backup and restore of both cached and stored volumes
- helps you centralize backup management, reduce your operational burden, and meet compliance requirements.

### cloudtrail

- Set up API and user activity logging with AWS CloudTrail.

### cloudwatch

- monitor your gateway and associated resources in CloudWatch by using Storage Gateway health and performance logs and metrics.
- Gateway management console provides a consolidated view of CloudWatch metrics and any CloudWatch alarms you have configured.
