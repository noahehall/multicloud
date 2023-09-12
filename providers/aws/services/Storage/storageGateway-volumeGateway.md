# Volume Gateway

- Hybrid cloud block storage with local caching using the iSCSI protocol.

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/volume/?nc=sn&loc=2&dn=4)
- [storage sizing](https://docs.aws.amazon.com/storagegateway/latest/vgw/ManagingLocalStorage-common.html)
- [CHAP](https://docs.aws.amazon.com/storagegateway/latest/vgw/GettingStartedConfigureChap.html)

## best practices

- volume sizes must be steady and predictable?
  - Volume resizing for the gateway is not supported
  - To decrease the storage capacity: create a new gateway and migrate your data to the new gateway
  - To increase storage capacity: add new disks to the gateway instead of expanding disks previously allocated.
- stored mode
  - processes that require reading all of the data on the entire volume such as virus scans?

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
- Using the iSCSI protocol, clients (called initiators) send Small Computer System Interface (SCSI) commands to storage devices (called targets) on remote servers
  - on-premises applications write data to and read data from a gateway's storage volume
  - this data is stored and retrieved from the volume's assigned disk.

#### Cached Volume mode

- Frequently accessed data is cached on premises for local use
  - primary data is stored in S3
  - Any data that your applications modify is moved to the upload buffer, where it is prepared for uploading asynchronously to S3.
- deployed into your on-premises or AWS Cloud environment as a VM running on VMware ESXi, KVM, Microsoft Hyper-V hypervisor, Amazon EC2, or a physical gateway hardware appliance
- range from 1 gibibyte (GiB) to 32 tebibytes (TiB) in size
  - support up to 32 volumes for a total maximum storage volume of 1,024 TiB [1 pebibyte (PiB)].
- Local disks are needed for cache storage and an upload buffer for a cached volume gateway.
- use cases
  - Custom file shares
  - Migrating application data into Amazon S3 and transitioning to Amazon EC2
  - applications where you have a large dataset, but your application only needs low-latency access to a subset of that data at any given time.

##### cache storage

- acts as the on-premises durable store for data that is pending upload to Amazon S3 from the upload buffer
- When applications performs I/O on a volume, the gateway saves the data to the cache storage for low-latency access
- when applications requests data from a volume, the gateway first checks the cache storage for the data before downloading the data from AWS.

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

### upload buffer

- Both cached volume and storage volume gateway deployments require upload buffer storage.
- provides a staging area for the data before the gateway uploads the data to Amazon S3
- the gateway uploads this buffer data over an encrypted SSL connection to AWS.

### snapshots

- volumes are asynchronously backed up as Amazon EBS snapshots and stored in an S3 service bucket
  - NOT of a customer bucket: they do not showup in the s3 console
  - are ONLY accessible from the Storage Gateway and Amazon EBS management console
- use it for disaster recovery using EBS snapshots or cached volume clones.

### Service Endpoints

- When Storage Gateway is deployed, it must communicate back to the Storage Gateway service for both management and data movement.
- Public endpoint: Storage Gateway connects to a public endpoint over the internet.
- VPC endpoint: Storage Gateway connects to Storage Gateway VPC endpoints over a private connection to AWS
  - via AWS Direct Connect or AWS VPN
- FIPS 140-2 compliant endpoints: Storage Gateway connects to a public endpoint over the internet.
  - Federal Information Processing Standards
  - This endpoint complies with FIPS standards, to further protect sensitive information for regulated workloads in AWS GovCloud (US) Regions.

#### Required Network Ports

- TCP 443: storage gateway VM -> AWS
- TCP 80: browser -> storage gateway VM
- UDP 53: storage gateway VM -> DNS Server
- TCP 22: Storage gateway VM -> AWS
- UDP 123: Storage gateway vm -> NTP server
- iSCI 3260: iSCI initiators -> storage gateway vm

### Challenge-Handshake Authentication Protocol (CHAP)

- provides protection against man-in-the-middle and playback attacks by periodically verifying the identity of an iSCSI initiator as authenticated to access a storage volume target
- For each volume target, you can define one or more CHAP credentials.

## considerations

- region: Storage Gateway is only available in select regions
- volume type
  - cached: only a subset of data is needed on premise
  - stored: the full dataset is needed on premise
- gateway appliance location
  - on premise: required for stored volumes
    - virtual machine (VM) appliance: download and deploy to:
      - VMware ESXi Hypervisor: Integrates with VMware vSphere High Availability (VMware HA)
      - Microsoft Hyper-V
      - Linux Kernel-based Virtual Machine (KVM)
    - purchase a hardware appliance
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
