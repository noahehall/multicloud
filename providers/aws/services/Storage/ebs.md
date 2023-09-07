# Elastic Block Store (EBS)

- high performance persistent network attached block storage for EC2 instances supporting throughput and transaction intensive workloads at any scale.

## my thoughts

- determine up front if you need multi attach to share data across EC2s as it depends on volume type
- remember there is a size limit for ebs, unlike S3

## links

- [landing page](https://aws.amazon.com/ebs/?did=ap_card&trk=ap_card)
- [intro](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)
- [faq](https://aws.amazon.com/ebs/faqs/)
- [eks: EBS CSI driver](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html)
- [eks: managing EBS CSI Addon](https://docs.aws.amazon.com/eks/latest/userguide/managing-ebs-csi-self-managed-add-on.html)
- [eks: EBS CSI driver github](https://github.com/kubernetes-sigs/aws-ebs-csi-driver)
- [data lifecycle manager](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/snapshot-lifecycle.html)
- [api reference](https://docs.aws.amazon.com/ebs/latest/APIReference/Welcome.html)

### tools

- [Flexible IO: load tester](https://github.com/axboe/fio)
- [io meter: load tester](http://www.iometer.org/doc/downloads.html)
- [sysbench: load tester](https://github.com/akopytov/sysbench)

## best practices

- ensure your taking regular snapshots, and removing old snapshots
- With Elastic Volumes,
  - volume sizes can only be increased within the same volumes
  - to decrease a volume size, you must copy the EBS volume data to a new smaller EBS volume.
- use Snapshots with automated lifecycle policies to back up your data to s3
- logically separate your backup data from your EBS volumes.
  - Create a separate AWS account for backups.
    - To copy EBS snapshots to a different account, you share the snapshots with that account by modifying the permissions.
  - protect backups from being renamed or encrypted by unauthorized users

### anti patterns

## features

- designed for scaling high performance workloads
- high availability, replication in/across AZs, 5 9s durability: automatically replicated in its availability zone
- build SAN in the cloud for i/o intensive apps
- data store for any database type
- runtime flexible: modify volume type/size, IOPS configuration, resize clusters for big data analytics engines
- opt-in data encryption for all volume types
- use cases: non/relational databases, enterprise/containerized applications, big data analytics engines, file systems, and media workflows.
  - operating systems: boot and root volumes can be used to store an OS
  - databases: the storage layer fo a DB running on EC2 that will scaled with performance needs
  - enterprise applications: EBS provides high availability and durability for mission critical applicatins
  - big data analytics engines: data persistence, dynamic performance adjustments, and the ability to de/reatch volumes when resizing clusters

### pricing

- FYI: the pricing structure is insane to calculate, use the pricing calculator
- volume type, provisioned volume size, and the provisioned IOPS and throughput performance
  - general purpose ssd: storage, iops, throughput, or regular volumes
  - provisioned IOPS ssd: storage, iops, or regular volumes
  - throughput optimized hdd: volumes
  - cold HDD: volumes
- snapshots also incur an additional charge
  - storage
    - standard
    - archive
  - restore
    - standard
    - archive
- theres additional costs for other operations
  - fast snapshot restore
  - direct apis

## basics

### EC2

- you need an EC2 instance in the same AZ to access data on an EBS volume
  - to associate the ebs with another ec2 instance
    - stop the current ec2 instance
    - detach the ebs volume
    - attach it to a different ec2 instance in the same AZ
  - this is a 1:M relationship: one ec2 can have multiple attached EBS volumes

### snapshots

- redundantly stored incremental backups:
  - new snapshots only track the blocks on the volume that have changed since the previous snapshot
  - backups are stored redundantly in multiple AZs using S3
- use cases: protects data with eleven 9's of durability and provides you Regional access and availability.
  - instantiate multiple new volumes in any AZ
  - expand the size of a volume
  - move volumes across Availability Zones
  - backup and retention

#### multi-volume snapshots

- critical workloads that spans across multiple EBS volumes
- take exact point-in-time, data-coordinated, and crash-consistent snapshots

#### creation

- create manual snapshots or create snapshot schedules
  - is constrained to the AWS Region where it was created
  - use it to create new volumes in the same Region
- Snapshots occur asynchronously
  - the point-in-time snapshot is created immediately,
  - the status of the snapshot is pending until the snapshot is complete
- modifying the permissions of a snapshot, share it with other AWS accounts
  - use the shared snapshots as the basis for creating their own EBS volumes.
  - can be modified by the authorized user; however, your original snapshot remains unaffected.

#### copy

- copy snapshots across/within AWS Regions
- copy any accessible snapshot that has a completed status.
- the copy receives an ID that is different from the ID of the original snapshot.

#### deleting snapshots

- delete any snapshot whether it is a full or incremental snapshot.
  - removes only the data not needed by any other snapshot.
- All active snapshots contain all the information needed to restore the volume to the instant at which that snapshot was taken

#### restoring snapshots

- the new volume begins as an exact replica of the original volume
- data is loaded into the new replicated volume in the background
- can begin to use your new volume immediately while the EBS volume data loads
  - accessing data that hasn't been loaded: the volume immediately downloads the requested data from Amazon S3.

#### Data Lifecycle Manager (DLM)

- automate the creation, retention, and deletion of snapshots that you use to back up your EBS volumes and EBS-backed AMIs
- create up to 100 lifecycle policies per AWS Region.
- add up to 45 tags per resource.
- Target resource tags are used to identify the resources to back up.
  - DLM tags are specific tags applied to all snapshots and AMIs created by a lifecycle policy.
    - distinguish them from snapshots and AMIs created by any other means.

##### Lifecycle Policies

- created using core policy settings to define the automated policy action and behavior.
- policy type: Defines the type of resources that the policy can manage.
  - Snapshot lifecycle policy: target EBS volumes and instances.
    - Cross-account copy event policy: automate the copying of snapshots across accounts; should be used in conjunction with an EBS snapshot policy that shares snapshots across accounts.
  - EBS-backed AMI lifecycle policy: can target instances only.
    - One AMI is created that includes snapshots of all of the volumes that are attached to the target instance.
- resource type: Defines the type of resources that are targeted by the policy.
  - Use VOLUME to create snapshots of individual volumes
  - use INSTANCE to create multi-volume snapshots of all of the volumes that are attached to an instance
- target tags: must be assigned to an EBS volume or an Amazon EC2 instance for it to be targeted by the policy.
- Schedules: start times and intervals for creating snapshots or AMIs.
  - operation starts within one hour after the specified start/schedule time
- retention: retain snapshots or AMIs based on their total count (count-based) or their age (age-based).
  - snapshot policies: the oldest snapshot is deleted.
  - AMI policies: the oldest AMI is deregistered and its backing snapshots are deleted.

##### Policy Schedules

- define when snapshots or AMIs are created by the policy.
- one mandatory schedule
- up to three optional schedules.
  - create snapshots or AMIs at different frequencies using the same policy
- for each schedule
  - frequency
  - lifecycle policies: fast snapshot restore settings
  - cross-Region copy rules
  - tags: automatically assigned to the snapshots or AMIs that are created

### volume types

- cab dynamically change the configuration of a volume attached to an EC2 instance
- Annual Failure Rate: AFR; 0.1 to 0.2 percent
- availabilty: 99.8-99.9:
- data is replicated across multiple servers in a single Availability Zone
- attach multiple EBS volumes during/after EC2 instance creation within the SAME AZ
- burstability
  - operate within your normal baseline range, you accumulate burst credits.
  - workload uses IOPS or throughput above your baseline range, you use your accumulated burst credits.
  - If your burst balance is depleted, you are unable to burst, and operations are limited to your provisioned baseline limits.
- elastic volumes:
  - increase and decrease provisioned performance settings
  - you can scale an ebs volume up to a max size of 64 tebibytes (TiB)
  - hange from one volume type to a different volume type.

#### SSD backed

- designed for transactional workloads
- I/O size is capped at 256 kibibyte (KiB)
- expected average latency ranges from sub-1 millisecond to single-digit millisecond performance depending on the SSD volume type.
- handle small or random I/O much more efficiently than HDD volumes

##### general purpose

- designed for large sequential workloads e.g. big data analytics engines, log processing, and data warehousing
  - balance of price and performance
- GP2: IOPS performance is tied to volume size,
  - performance scales linearly at 3 IOPS per GiB of volume size.
    - Larger volumes have higher baseline performance levels and accumulate I/O credits faster.
  - minimum of 100 IOPS at 33.33 GiB and below to a maximum of 16,000 IOPS at 5,334 GiB and above.
  - volume size can range from 1 GiB to 16 TiB.
- GP3: scale IOPS and throughput independent from the volume size.
  - workloads performing small, random I/O.
  - 3,000 IOPS and 125 megabytes per second (MB/s) of throughput
  - independently provision additional performance up to a total of 16,000 IOPS and 1,000 MB/s throughput for an additional cost.
  - estimated to be appropriate for up to 80 percent of the workloads.

##### provisioned IOPS

- high performance for mission-critical, low-latency, or high-throughput workloads.
  - particularly database workloads,
- volume size
  - 4 GiB to 16 TiB. You can provision 100–64,000 IOPS per volume on instances built on the Nitro System and up to 32,000 on other instances.
- io1:
  - 99.8–99.9 percent volume durability with an AFR no higher than 0.2 percent
  - available for all Amazon EC2 instance types.
- io2: the most current Provisioned IOPS SSD volumes available and are recommended by AWS for all new deployments.
  - 99.999 percent volume durability with an AFR no higher than 0.001 percent
  - available for all EC2 instances types, with the exception of R5b.
- io2 block express: abcd
- EBS multi attach: only for io1 & io2 volume types
  - allows a single EBS volume to be concurrently attached to up to 16 Nitro-based EC2 instances within the same Availability Zone.
  - to achieve higher application availability for applications that manage storage consistency from multiple writers
  - Applications using multi attach need to provide I/O fencing for storage consistency.

#### HDD backed

- I/O size is capped at 1,024 KiB
- expected average latency is two-digit millisecond performance
- designed to deliver their provisioned performance 90 percent of the time.
- volume size can range from 125 GiB to 16 TiB.

##### throughput optimized

- frequently accessed, throughput-intensive workloads
- st1: low-cost magnetic storage that defines performance in terms of throughput rather than IOPS
  - support frequently accessed data
  - optimized for workloads involving large, sequential I/O; e.g. Amazon EMR, data warehouses, log processing, and extract, transform, and load (ETL) workloads.

##### cold

- throughput-oriented storage for data that is infrequently accessed and scenarios where the lowest storage cost is important.
- sc1: a bunch of iops stuff thats lame relative to the others

#### Magnetic (standard)

- previous-generation EBS volume type that is still in use in some customer production environments and available on AWS Management Console.

### Data Lifecycle Manager

- automated ebs snapshot management

### Security

#### Encryption

- encryption can be enabled by default at the account level
  - any new volumes will be automatically encrypted
- the encryption data key is stored on-disk with your encrypted data,
  - but not before EBS encrypts it with your CMK
  - data key
    - never appears on disk in plaintext.
    - is shared by snapshots of the volume and any subsequent volumes created from those snapshots
- at rest: using KMS or customer managed keys
  - data volumes:
  - boot volumes: by default are NOT encrypted; enable when creating the AMI/modify default settings
  - snapshots:
- in transit: encryption occurs on the servers that host EC2 instances before sending it to EBS
- snapshot encryption rules:
  - Snapshots of encrypted volumes are automatically encrypted.
  - Volumes created from
    - encrypted snapshots are automatically encrypted.
    - an unencrypted snapshot can be encrypted during the creation process.
  - copy an
    - unencrypted snapshot, you can encrypt it during the copy process.
    - encrypted snapshot, you can re-encrypt it with a different encryption key during the copy process.
  - The first snapshot taken of
    - an encrypted volume that was created from an unencrypted snapshot is always a full snapshot.
    - a re-encrypted volume that has a different encryption key from the source snapshot is always a full snapshot.
- the following types of data are encrypted:
  - Data at rest inside the volume
  - All data moving between the EBS volume and the EC2 instance
  - All snapshots created from the EBS volume
    - All EBS volumes created from those snapshots

## considerations

## integrations

- use the same connectivity and access methods that you use to reach your EC2 instances to reach your EBS volumes.

### AMI

- EBS-backed AMIs: the root instance launched from an AMI is typically an EBS volume

### EKS

- integrates with k8s via an EBS CSI driver
- use cases
  - application workloads deployed into a k8s statefulset object
- general workflow
  - cluster user submits a peristent volume claim (PVC)
  - the EBS storage class calls the EBS CSI driver to allocate storage matching the PVC request
  - the EBS CSI driver makes aws api calls to create an EBS volume and attach it to the requested cluster node
  - on success: the persistent volume (PV) is allocated to the PVC
- EBS CSI Driver
  - must be deployed to AWS: e.g. via a helm chart or yaml manifest file
    - this shiz is hella involved, google how to do it when you need it
  - can be configured to use various EBS functionality
    - volume resizing
    - snapshots
    - etc
  - requires explicit permissions to access the AWS apis
    - you need to attach an IAM policy with the correct role attached to the CSI driver service account
      - you can do this via `eksctl`

### Backup

- centralize and automate data protection across multiple Amazon EBS volumes

### Cloudwatch

- Snapshot events are tracked through CloudWatch events
- An event is generated each time you create a single snapshot or multiple snapshots, copy a snapshot, or share a snapshot.
