# Elastic Block Store (EBS)

- high performance persistent network attached block storage for EC2 instances supporting throughput and transaction intensive workloads at any scale.
- [tags](../globalInfrastructure/tags.md)

## my thoughts

- remember there is a size limit for ebs, unlike S3

## links

- [AAA api reference](https://docs.aws.amazon.com/ebs/latest/APIReference/Welcome.html)
- [benchmarking](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/benchmark_procedures.html)
- [concepts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html)
- [data lifecycle manager](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/snapshot-lifecycle.html)
- [designing for performance VIDEO](https://www.youtube.com/watch?v=2wKgha8CZ_w)
- [dlm: landing page](https://aws.amazon.com/ebs/data-lifecycle-manager/)
- [ec2: volume limits](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/volume_limits.html)
- [ecs: using volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
- [eks: EBS CSI driver github](https://github.com/kubernetes-sigs/aws-ebs-csi-driver)
- [eks: EBS CSI driver](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html)
- [eks: managing EBS CSI Addon](https://docs.aws.amazon.com/eks/latest/userguide/managing-ebs-csi-self-managed-add-on.html)
- [faq](https://aws.amazon.com/ebs/faqs/)
- [i/o characteristics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-io-characteristics.html)
- [intro](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)
- [landing page](https://aws.amazon.com/ebs/?did=ap_card&trk=ap_card)
- [monitoring](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-volume-status.html)
- [optimize provisioned iops](https://aws.amazon.com/premiumsupport/knowledge-center/optimize-ebs-provisioned-iops/)
- [quotas](https://docs.aws.amazon.com/general/latest/gr/ebs-service.html)
- [raid](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/raid-config.html)
- [snapshots: copy](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-copy-snapshot.html)
- [snapshots: crash consistent](https://aws.amazon.com/blogs/storage/taking-crash-consistent-snapshots-across-multiple-amazon-ebs-volumes-on-an-amazon-ec2-instance/)
- [snapshots: creating](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-snapshot.html)
- [snapshots: deleting](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-snapshot.html)
- [snapshots: FSR](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-fast-snapshot-restore.html)
- [volume: monitoring](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/monitoring-volume-status.html)
- [volumes: elastic](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modify-volume.html)
- [volumes: initialization](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-initialize.html)
- [volumes: multi attach](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes-multi.html)
- [volumes: types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html)

## best practices

- ensure your taking regular snapshots, and removing old snapshots
- use Snapshots with DLM lifecycle policies to back up your data to s3
- logically separate
  - backup data from your EBS volumes.
    - Create a separate AWS account for backups.
      - To copy EBS snapshots to a different account, you share the snapshots with that account by modifying the permissions.
    - protect backups from being renamed or encrypted by unauthorized users
  - root volume from app data volumes
    - each can be detached and reattached without them interfering with each other.
- controlling costs:
  - Delete inactive or unattached EBS volumes and old snapshots when appropriate.
    - DeleteOnTermination flag
    - Deleting a snapshot has no effect on the volume.
    - Deleting a volume has no effect on the snapshots made from it.
  - Avoid provisioning EBS volumes larger than required.
  - Avoid over-sizing provisioned performance options.
  - Use newer volume types when appropriate.
  - Use lower-cost volume types when appropriate.
  - opportunities with EBS Snapshots
    - Design your DLM policies to keep only the snapshots that you need to keep.
    - Use EBS Snapshots Archive to create archival snapshots.
    - Use AWS Backup to create backup copies of your data to maintain archival data copies rather than keeping additional snapshots.
- volume types supporting provisioning IOPS performance separately from the volume size
  - io1, io2, and gp3
- to calculate minimum volume size to support a sustained IOPS target for General Purpose and Provisioned IOPS volumes
  - Required minimum sustained IOPS
  - IOPS-to-volume capacity ratio
- Monitor the read/write access of EBS volumes to determine if throughput is low.
- perf recommendations
  - For maximum consistency, a Provisioned IOPS volume must maintain an average queue depth of one for every 1000 provisioned IOPS in a minute (rounded to the nearest whole number).
    - e.g. for a volume provisioned with 3000 IOPS, the queue depth average must be 3.
  - By creating a RAID 0 array, you can achieve a higher level of performance for a file system than you can provision on any single EBS volume.
  - use EBS optimized instances
  - consider
    - using additional non-root EBS volumes for your applications, or for the different functions of a single application.
    - using the root volume for the operating system only.

### anti patterns

- IOPS and throughput limitations.
  - SSD: large I/O sizes may experience a smaller number of IOPS than you provisioned because you are hitting the throughput limit for the volume.
  - HDD: used for small or random I/O workloads can experience a lower throughput than expected
    - EBS counts each random, non-sequential I/O towards the total IOPS count, which can cause you to hit the volume's IOPS limit sooner than expected.

## features

- designed for scaling high performance workloads
- high availability, replication in/across AZs, 5 9s durability: automatically replicated in its availability zone
- build SAN in the cloud for i/o intensive apps
- data store for any database type
- runtime flexible: modify volume type/size, IOPS configuration, resize clusters for big data analytics engines
- opt-in data encryption for all volume types
- use cases: non/relational databases, enterprise/containerized applications, big data analytics engines, file systems, and media workflows.
  - operating systems: boot and root volumes
  - databases: the storage layer fo a DB running on EC2 that will scaled with performance needs
  - enterprise applications: EBS provides high availability and durability for mission critical applicatins
  - big data analytics engines: data persistence, dynamic performance adjustments, and the ability to de/reatch volumes when resizing clusters

### pricing

- FYI:
  - remember that you are still charged for volumes that are unused and unattached
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
- decision tree
  - first consider your latency and data durability requirements
  - Then you can consider your performance requirement
    - performance profile requirement for IOPS?
    - performance profile requirement for throughput?
    - performance profile requirement for IOPS?

## basics

### Lifecycle

#### Creation

- New volumes are simply raw block devices.
- you must first format & create a file system on them before use
  - The formatting process overwrites any existing data and sets up a file system
- volumes from snapshots
  - the volume is created in the background so you don't need to wait for all of the data to transfer from Amazon S3 to your Amazon EC2 instance before you can access the volume.

#### Available

- unattached EBS volumes

### Block Devices

- root volumes: contains all the necessary data required to boot an instance
  - are automatically attached to intances
  - windows instances require EBS volumes as root volumes
- data volumes: where application data is stored
- block device mappings: defines the block devices attached to an instance.

#### Lifecycle

##### Termination

- by default, EBS volumes are deleted when the associated ec2 instances are terminated
  - root volumes: YES
  - data volumes: NO

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
- elastic volumes: current-generation EBS volume attached to a current-generation EC2 instance type
  - increase and decrease provisioned performance settings
  - you can scale an ebs volume up to a max size of 64 tebibytes (TiB)
  - hange from one volume type to a different volume type.

#### SSD backed

- designed for transactional workloads involving frequent read/write operations with small I/O size, where the dominant performance attribute is IOPS.
- I/O size is capped at 256 kibibyte (KiB)
- expected average latency ranges from sub-1 millisecond to single-digit millisecond performance depending on the SSD volume type.
- handle small or random I/O much more efficiently than HDD volumes

##### general purpose

- designed for large sequential workloads e.g. big data analytics engines, log processing, and data warehousing
  - balance of price and performance
  - boot volumes, and dev/test environments, Virtual desktops, medium-sized single instance databases
- GP2: IOPS performance is tied to volume size,
  - performance scales linearly at 3 IOPS per GiB of volume size.
    - i.e. performance is tied to volume size, which determines the baseline performance level of the volume and how quickly it accumulates I/O credits.
    - Larger volumes have higher baseline performance levels and accumulate I/O credits faster.
  - minimum of 100 IOPS at 33.33 GiB and below to a maximum of 16,000 IOPS at 5,334 GiB and above.
  - volume size can range from 1 GiB to 16 TiB.
  - Your costs are based on the provisioned volume capacity.
  - optimize for capacity so that you’re paying only for what you use.
- GP3: scale IOPS and throughput independent from the volume size.
  - lowest cost SSD volume
  - workloads performing small, random I/O.
  - 3,000 IOPS and 125 megabytes per second (MB/s) of throughput
  - independently provision additional performance up to a total of 16,000 IOPS and 1,000 MB/s throughput for an additional cost.
  - estimated to be appropriate for up to 80 percent of the workloads.
  - Your costs are based on the provisioned volume capacity plus the provisioned IOPS above 3,000 IOPS and provisioned throughput above 125 MB/s.

##### provisioned IOPS

- high performance for mission-critical, low-latency, or high-throughput workloads.
  - particularly database workloads,
- volume size
  - 4 GiB to 16 TiB. You can provision 100–64,000 IOPS per volume on instances built on the Nitro System and up to 32,000 on other instances.
- io1: Highest performance SSD volume; I/O-intensive NoSQL and relational databases
  - 99.8–99.9 percent volume durability with an AFR no higher than 0.2 percent
  - available for all Amazon EC2 instance types.
  - Your costs are based directly on the provisioned volume capacity plus the provisioned IOPS.
    - Provisioned IOPS are charged at a flat rate up to the maximum of 64,000 IOPS.
    - pay close attention to IOPS use rather than throughput,
      - Provision 10–20 percent above maximum IOPS utilization.
  - adjust IOPS performance without detaching the volume.
  -
- io2: Highest performance and highest durability SSD volume; I/O-intensive NoSQL and relational databases
  - the most current Provisioned IOPS SSD volumes available and are recommended by AWS for all new deployments.
  - 99.999 percent volume durability with an AFR no higher than 0.001 percent
    - all other volume types provide 99.8-99.9 % durability with an AFR of between 0.1–0.2 percent.
  - available for all EC2 instances types, with the exception of R5b.
  - Your costs are based on the provisioned volume capacity plus the provisioned IOPS.
    - Provisioned IOPS are charged using a tiered structure.
      - Tier 1 is up to 32,000 IOPS.
      - Tier 2 is 32,001 to 64,000 IOPS,
      - Tier 3 is over 64,000 IOPS.
- io2 block express: highest perf ssd volume; Largest, most I/O-intensive, mission-critical deployments of db engines
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
  - support frequently accessed, throughput-intensive workloads
  - optimized for workloads involving large, sequential I/O; e.g. Amazon EMR, data warehouses, log processing, and extract, transform, and load (ETL) workloads.

##### cold

- throughput-oriented storage for data that is infrequently accessed and scenarios where the lowest storage cost is important.
- sc1: less frequently accessed workloads

#### Magnetic (standard)

- previous-generation EBS volume type that is still in use in some customer production environments and available on AWS Management Console.

### snapshots

- redundantly stored incremental backups:
  - new snapshots only track the blocks on the volume that have changed since the previous snapshot
- backups are stored redundantly in multiple AZs using S3
- use less storage and have a lower cost than Amazon EBS volumes.
- use cases: protects data with eleven 9's of durability and provides you Regional access and availability.
  - instantiate multiple new volumes in any AZ
  - expand the size of a volume
  - move volumes across Availability Zones
  - backup and retention
- There is a significant increase in latency when you first access each block of data on a new EBS volume that was created from a snapshot.
  - initialization: Access each block before putting the volume into production
  - fast snapshot restore: does this for you; see below

#### consistency

- crash consistent: contains all transactions that were written to the volume before the snapshot was taken
  - Any information in memory or any transactions not committed to disk at the time of the snapshot are discarded.
- application consistent: ensure that the application transactions are completed and written to the volume before the snapshot is taken
  - include data from pending transactions between these applications and the disk
- multi volume, crash consistent: create multi volume snapshots, which are point-in-time snapshots for all EBS volumes attached to an EC2 instance
  - typically restored as a set.
    - recommend tagging your multiple-volume snapshots to manage them collectively during restore, copy, or retention
  - After the snapshots are created, each snapshot is treated as an individual snapshot.
  - multi volume snapshots support up to 40 EBS volumes per instance.

#### Snapshots Archive

- store archival copies of your snapshots for retention purposes
- billed based on the amount of data in GB that your snapshots consume and the AWS Region
  - approximately one quarter of the cost of standard EBS Snapshot storage costs.

#### multi volume snapshots

- point-in-time, data-coordinated, and crash-consistent snapshots for all EBS volumes attached to an EC2 instance
  - support up to 40 EBS volumes per instance.
  - If any one snapshot for the multi-volume snapshot set fails, all of the other snapshots display an error status and a create a Snapshots CloudWatch event.
- use cases
  - critical workloads that spans across multiple EBS volumes
  - tagging multiple volume snapshots to manage them collectively during restore, copy, or retention
    - automatically copy tags from source volume to snapshots
    - set the snapshot metadata, such as access policies, attachment information, and cost allocation, to match the source volume.
    - snapshots for Amazon EBS volumes that are configured in a RAID array

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
- All active snapshots contain all the information needed to restore the volume to the instant at which that snapshot was taken

#### copy

- copy snapshots across/within AWS Regions once the status is Completed
  - i.e. has finished copying to Amazon S3
- the copy receives an ID that is different from the ID of the original snapshot.
- S3 server-side encryption (256-bit AES) protects a snapshot's data in transit during a copy operation.
- multi-volume snapshots
  - retrieve the snapshots using the tag you applied to the multi-volume snapshot set when you created it. Then, individually copy the snapshots to another Region.

#### deleting

- deleting any snapshot whether it is a full or incremental snapshot.
  - removes only the data not needed by any other snapshot.
- Check for snapshots that are over 30 days old and delete them to reduce storage costs

#### restoring

- the new volume begins as an exact replica of the original volume
- data is loaded into the new replicated volume in the background
- can begin to use your new volume immediately while the EBS volume data loads
  - accessing data that hasn't been loaded: the volume immediately downloads the requested data from Amazon S3.

#### Data Lifecycle Manager (DLM)

- automate the creation, retention, and deletion of snapshots that you use to back up your EBS volumes and EBS-backed AMIs
- create up to 100 lifecycle policies per AWS Region.
- add up to 45 tags per resource.
- make sure you read the tags file as DLM is heavily dependent on tags

##### Lifecycle Policies

- created using policy settings to define the automated policy action and behavior.
- policy type: Defines the type of resources that the policy can manage.
  - Snapshot lifecycle policy: automate the lifecycle of EBS snapshots.
    - target EBS volumes and instances.
  - Cross-account copy event policy: automate the copying of snapshots across accounts
    - used in conjunction with an EBS snapshot policy that shares snapshots across accounts.
  - EBS-backed AMI lifecycle policy: automate the lifecycle of EBS-backed AMIs
    - can target instances only.
    - One AMI is created that includes snapshots of all of the volumes that are attached to the target instance.
- resource type: Defines the type of resources that are targeted by the policy.
  - VOLUME: to create snapshots of individual volumes
  - INSTANCE: to create multi volume snapshots of all of the volumes that are attached to an instance
- target tags: Specifies the tags that must be assigned to an EBS volume/ec2 instance for it to be targeted by the policy.
  - target an instance or volume for backup using a single tag.
  - Multiple tags can be assigned to an instance or volume if you want to run multiple policies on it.
- Schedules: start times and intervals for creating snapshots or AMIs.
  - operation starts within one hour after the specified start/schedule time
- retention: retain snapshots or AMIs based on their total count (count-based) or their age (age-based).
  - snapshot policies: the oldest snapshot is deleted.
  - AMI policies: the oldest AMI is deregistered and its backing snapshots are deleted.

##### Policy Schedules

- define when snapshots or AMIs are created by the policy.
- one mandatory & up to three optional schedules.
  - create snapshots or AMIs at different frequencies using the same policy
- for each schedule
  - frequency
  - lifecycle policies: fast snapshot restore settings
  - cross-Region copy rules
  - tags: automatically assigned to the snapshots or AMIs that are created

#### Fast Snapshot Restore (FSR)

- create a volume from a snapshot that is fully initialized at creation, eliminating the latency when accessing it for the first time
- must be explicitly enabled per snapshot per AZ; else the volume is lazy loaded with data from the snapshot
- enable up to 50 snapshots for FSR per Region.
  - applies to snapshots that you own and snapshots that are shared with you
- volume creation credits
  - Volumes created from an FSR-enabled snapshot are fully initialized.
  - there are limits on the number of volumes that can be created with immediate full performance.
    - A single volume create operation consumes a single credit.
    - The number of credits is a function of the FSR-enabled snapshot size.
    - Credits refill over time.
    - The maximum credit bucket size is 10.
    - credits are not shared between snapshots

### RAID

- In traditional on-premises data centers, RAID arrays write data across multiple disks.
  - When used with Amazon EBS, RAID writes across multiple volumes
- is accomplished at the software level, use any RAID configuration supported by the operating system for your instance
- any RAID level is supported but only RAID 0 is recommended
- Some instance types can drive more I/O throughput than what you can provision for a single EBS volume.
  - join multiple volumes together in a RAID 0 configuration to use the available bandwidth for these instances.

### monitoring

- detect issues with volume inconsistencies, space utilization, and performance bottlenecks. Visibility into resource use helps when optimizing volume sizes and determining storage capacity needs.
- I/O characteristics drive the performance behavior of the volume

#### volume status checks

- automated tests that run every 5 minutes
  - determine whether your Amazon EBS volumes are healthy or impaired
- states
  - Okay: all checks passed
  - Warning: degraded; performance is below expectations
  - Impaired: one/more checks failed
  - Insufficient-Data: tests may still be running
- remediation:
  - the default option is to disable I/O to the volume from any attached EC2 instances
    - an event alerts you that I/O is disabled and that you can resolve the impaired status of the volume by enabling I/O to the volume.
  - you can override the default behavior by configuring the volume to automatically enable I/O
    - an event will alert you that the volume was determined to be potentially inconsistent, but that its I/O was automatically enabled

#### EC2 console dashboard events

- Awaiting Action Enable IO: volume data is potentially inconsistent. I/O is disabled for the volume until you explicitly enable it.
- IO Enabled: I/O operations were explicitly enabled for this volume.
- IO Auto-Enabled: I/O operations were automatically enabled on this volume after an event occurred.
  - check for data inconsistencies before continuing to use the data.
- For io1, io2, and gp3 volumes only.
  - Normal: Volume performance is as expected.
  - Degraded: Volume performance is below expectations.
  - Severely Degraded: Volume performance is well below expectations.
  - Stalled: Volume performance is severely impacted.

#### Considerations

- IOPS: I/O size is capped at
  - ssd: 256 KiB
  - hdd: 1,024
    - EBS divides large sequential I/O operations into separate I/O operations up to the maximum I/O size.
    - A single 1,024 KiB operation would count as four operations on SSDs and one operation on HDDs.
- block size: Amazon EBS advertises 512-byte sectors to the OS, which then reads and writes data to disk using data blocks that are a multiple of the sector size.
- throughput:
  - gp2: `(Number of IOPS) * (size per I/O operation)`
    - up to the throughput limit of 250 MiB/s
- latency: The expected average latency ranges from
  - SSD: sub-1 millisecond to single-digit millisecond performance depending on the SSD volume type.
  - HDD: two-digit millisecond performance
- performance tuning
  - BurstBalance: Displays the burst bucket balance for gp2, st1, and sc1 volumes as a percentage of the remaining balance.
  - VolumeReadByte
  - VolumeWriteBytes
  - VolumeReadOps
  - VolumeWriteOps
  - VolumeQueueLength
- performance options
  - file system mount options
  - block alignment
  - volume type, size, provisioned IOPS, maximum throughput, etc

### Security

#### Encryption

- check the KMS file

## considerations

- DeleteOnTermination: controls the default behavior of EBS deletion when associated EC2 instances are terminated
- ec2
  - volume limits
    - e.g. elastic volume aggregated storage
  - ebs & ebs types are compatible
  - ebs volumes with market place codes
    - volume can only be attached to a stopped instance.
    - must be subscribed to the AWS Marketplace code that is on the volume.
    - Marketplace product codes are copied from the volume to the instance.

## integrations

- use the same connectivity and access methods that you use to reach your EC2 instances to reach your EBS volumes.

### EC2

- you need an EC2 instance in the same AZ to access data on an EBS volume
  - to associate the ebs with another ec2 instance
    - stop the current ec2 instance
    - detach the ebs volume
    - attach it to a different ec2 instance in the same AZ
  - this is a 1:M relationship: one ec2 can have multiple attached EBS volumes
- verify that the instance supports the volume type that you want to create

#### multi attach

- can attach a single Provisioned IOPS SSD (io1 or io2) volume to multiple instances
  - only available in certain regions
  - Do not support I/O fencing or being boot volumes.
  - EBS volume and instance must be in the same Availability Zone.
  - cannot be enabled during instance launch using either the Amazon EC2 console or RunInstances API.
- nitro instances: up to 27 volumes
- linux instances: some support up to 40 volumes
- Using Multi-Attach with a standard file system can result in data corruption or loss and should not be used with production workloads
  - use a clustered file system to ensure data resiliency and reliability for production workloads.

#### elastic volumes

- enable runtime modifications of volume characteristics without detaching
  - All current-generation ec2 instances
    - previous-generation instances: C1, C3, CC2, CR1, G2, I2, M1, M3, and R3.
  - unsupported instance types: you must stop the instance, modify the volume, and then restart the instance.
    - then check the file system size to see if your instance recognizes the larger volume space.
    - else you must extend the file system of the device so that your instance can use the new space.
- check the docs for other requirements: theres some specific to windows/linux
  - increase the volume size, change the volume type, or adjust the performance
    - volume sizes can only be increased within the same volume
      - to decrease a volume size, you must copy the EBS volume data to a new smaller EBS volume.
    - new volume size can't exceed the supported capacity of its file system and partitioning scheme
  - After modifying a volume, you must wait at least six hours and ensure that the volume is in the in-use or available state before you can modify the same volume.
- modification life cycle:
  - modifying:
    - Size changes usually take a few seconds to complete and take effect after the volume has transitioned to the optimizing state.
    - Performance (IOPS) changes can take from a few minutes to a few hours to complete and depend on the configuration change being made.
      - up to 24 hours for a new configuration to take effect
      - e.g. a fully used 1-TiB volume takes about 6 hours to migrate to a new performance configuration.
  - optimizing: volume performance is in between the source and target configuration specifications.
  - completed:

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
- backup vs DLM
  - dlm: use DLM when you want to automate the creation, retention, and deletion of EBS snapshots.
    - creates lifecycle policies to remove old snapshots.
  - use AWS Backup to manage and monitor backups across the AWS services you use, including EBS volumes, from a single place.
    - set a retention policy to remove old snapshots.

### Cloudwatch

- Snapshot events are tracked through CloudWatch events
- An event is generated each time you create a single snapshot or multiple snapshots, copy a snapshot, or share a snapshot.

### EventBridge

### compute optimizer

- Once your EBS volumes are in operation, monitor them and verify that your volumes are providing optimal performance and cost effectiveness using AWS Compute Optimizer.

### Trusted Advisor

- Trusted Advisor will automatically find and report unattached volumes

### cloudtrail

### lambda

### step functions

- ondemand performance tuning: send Systems Manager commands that run OS-level scripts
- CloudWatch to monitor the capacity of the volume, you can configure a series of Step Functions
  - run checks on your EC2 instances,
  - snapshot the volume as a temporary backup,
  - expand the volume and extend the file system,
  - verify the successful status of the volume and file system extension,
  - then notify your IT staff when the process is complete.

### Tags
