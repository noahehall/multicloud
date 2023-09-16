# File Gateway: s3 & FSx

- Store and access SMB and NFS file data with local caching
- S3 and FSx file gateways

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/file/?nc=sn&loc=2&dn=2)
- [s3: intro](https://aws.amazon.com/storagegateway/file/s3/)
- [fsx: intro](https://aws.amazon.com/storagegateway/file/fsx/)
- [requirements](https://docs.aws.amazon.com/filegateway/latest/files3/Requirements.html)
- [s3: concepts](https://docs.aws.amazon.com/filegateway/latest/files3/StorageGatewayConcepts.html)
- [s3: nfs file share settings](https://docs.aws.amazon.com/filegateway/latest/files3/edit-storage-class.html)
- [s3: smb file share settings](https://docs.aws.amazon.com/filegateway/latest/files3/edit-smbfileshare-settings.html)

## best practices

- s3 vs FSx file gateways
  - s3: move your file data into an object format that is highly durable and cost efficient
  - FSx: keep it stored natively as file data
- size of the cache storage:
  - a larger local cache means that more data is readily available on premises, decreasing the cost of retrieving the data from Amazon S3
  - should allocate at least 20 percent of your existing file store size as cache storage
- optimize gateway performance
  - add high-performance disks such as solid state drives (SSDs) or an NVMe controller, or attach a virtual disk to your VM directly.
- write objects directly from a file share to the S3 Standard storage class and use a lifecycle policy to transition the objects to other storage classes
- multiple gateways or file shares write to the same S3 bucket, unpredictable results might occur.
  - Configure your S3 bucket so that only one file share can write to it.
    - Permit multiple Network File System (NFS) clients to write to the same S3 bucket through a single S3 File Gateway.
    - S3 bucket policy that denies all roles, except the role used for the specific file share, to put or delete objects in the bucket and attach this policy to the S3 bucket.
    - best practice for secondary gateways is to either use an IAM role that is prevented from writing to the bucket or permit the export of file shares as read-only
  - If you want to write to the same Amazon S3 bucket from multiple file shares, you must prevent the file shares from trying to write to the same objects simultaneously
    - configure a separate, unique object prefix for each file share
      - each file share will only write to objects with its corresponding prefix

### anti patterns

## features

### pricing

## basics

### s3 file gateway

- used to store NFS and SMB files as native s3 objects for data lakes, backups, and ML workflows.
  - accessbile over NFS, SMB and directly via s3 API
  - POSIX-style metadata: including ownership, permissions, and timestamps, are durably stored in Amazon S3 in the user-metadata of the object associated with the file.
  - implement storage management capabilities, such as versioning, lifecycle management, and cross-Region replication.
  - Use up to 64 TB of cache per gateway, and set up automatic cache refresh at 5-minute intervals.
    - cache will be used by all the file shares on the S3 File Gateway appliance.
    - When adding cache to an existing gateway, create new disks in your host. Do not change the size of previously allocated disks.
- storage is made accessible to database and application servers as locally mountable file shares, using industry-standard file protocols:
  - Linux clients: NFS (versions 3 and 4.1)
  - Windows clients: SMB (versions 2 and 3)

#### Provisioning

- After you deploy the Storage Gateway appliance, you activate it and configure its storage
- connect the hosted S3 File Gateway appliance to AWS through a service endpoint
  - publicly accessible endpoint: optionally FIPS-enabled endpoint if required
  - VPC hosted: an existing VPC endpoint, or provide its Domain Name System (DNS) name or IP address.
- specify how your deployed gateway appliance will be identified in the connection and securely associated with your AWS account.
  - provide an IP address that is either public or accessible from within your current network.
  - generate an activation key using the gateway's local console and use that instead of the IP address.
- Configure local disks
  - on premise: allocate a local disk as the cache of the S3 File Gateway and configure your Storage Gateway to use this disk as cache.
- After you create and activate the S3 File Gateway, you can create file shares that can be mounted on your Linux or Windows servers

#### Files

##### file share

- Each file share is associated with a unique S3 bucket or a unique prefix on the same bucket.
  - Each gateway is limited to 10 shares.
  - Each file share needs to be connected to an S3 bucket and given access to the bucket through an IAM role with a trust policy
- A specific S3 File Gateway appliance can have multiple NFS and SMB file shares.
- storage class for your objects: S3 Standard, S3 Intelligent-Tiering, S3 Standard-Infrequent Access (S3 Standard-IA), and S3 One Zone-Infrequent Access (S3 One Zone-IA).
- Files written to the file share become objects in the S3 bucket, with a one-to-one mapping between files and objects.
  - Metadata, such as ownership and timestamps, are stored with the object
  - File paths become part of the object's key, and thus maintain consistent name space
  - Objects in Amazon S3 appear as files to the on-premises clients.
- like all other gateway types, you can deploy
  - on premise: hardware appliance / VM Host
  - in the cloud on an EC2
- file share status: available, creating, updating, deleting, force_deleting, unavailable
- file share actions: refresh schedule, upload notifications,

##### Reads

- client reads data from the gateway appliance using the NFS or SMB protocols.
  - first check to see if the data is in the cache
    - If the data is not in the cache, it's fetched from the S3 bucket using byte-range gets to better use available bandwidth.
      - gateway service retrieves the data from Amazon S3 and sends it to the gateway appliance.
      - The gateway appliance receives the data, stores it in the local cache, and provides it to the client.

##### Writes

- Write requests from the client are written to the cache first and are then asynchronously uploaded to the S3 bucket using either the NFS or SMB protocols
  - gateway stores the data locally.
  - data is compressed asynchronously, and changed data is uploaded securely
  - Changes to the files asynchronously update the objects in the S3 bucket
    - Multipart uploads and parallelism are used for uploads from the gateway cache to AW
- Uploading the file data creates an S3 object, and uploading the metadata for the file updates the metadata for the S3 object
  - This process creates another version of the object, resulting in two versions
  - If versioning is enabled for the S3 bucket, both versions of the object will be stored.
- if a file is modified after it has been uploaded, the modification will also result in multiple versions of the S3 object.
  - S3 File Gateway uploads the new or modified data instead of uploading the whole file and also uploads its metadata.
- renaming a file will replace the existing object and create a new S3 object
  - Depending on the S3 storage class being used, early deletion fees and retrieval fees might apply.

#### gateway: cloud / on premise VM + hardware appliance

- is what provides access to objects in Amazon S3 as files using NFS or SMB protocols
- on premise: the gateway provides a local point of presence through a cache that stores data for low-latency access.
  - also stores a local inventory of the objects in the S3 bucket and the metadata.
    - used to provide low-latency access for file system operations; for example, listing an inventory.
- cache refresh: refresh the inventory of objects that your gateway is aware of
  - find objects in the S3 bucket that were added, removed, or replaced since the gateway last listed the bucket's contents and cached the results
  - can configure automated cache refresh based on a timer value between 5 minutes and 30 days.
    - can also be refreshed using the RefreshCache API operation

### FSx File gateway

- provides your applications a file interface to seamlessly and durably store files as objects in Amazon S3.
- solution for replacing on-premises NAS, such as end-user home directories and departmental or group servers, with cloud storag
  - user or team file shares, and file-based application migrations like web content management, and media workflows.
  - migrate and consolidate on-premises file-based application data stored on NAS arrays or file server virtual machines into FSx
- provides low-latency, on-premises access to fully managed file shares & home directories in FSx Windows File Server.
  - store and access file data with Windows-native compatibility, including full NTFS support, shadow copies, and access control lists (ACLs).

### security

- To restrict file access for your NFS and SMB clients, you can edit the file share access setting within the Storage Gateway console.
  - nfs: By default, any client on your network can mount to your file share
    - Limit access to specific NFS clients or networks by IP address.
    - Permit read-only or read-write access.
    - Activate user permission squashing.
  - smb: set the security level for your gateway by doing the following:
    - Limiting access for Active Directory users only or providing authenticated guest access to users
    - Setting file share visibility for your file share to one of the following: read-only / read-write
    - Controlling file or directory access by POSIX, or, for fine grained permissions, using Windows ACLs
      - If you configure guest access authentication, POSIX is used for permissions.
- Access to Amazon S3: The gateway uses an S3 bucket. The content of this bucket can also be accessed by other workflows
  - IAM User Policies
  - bucket policies
  - s3 block public access
  - activating S3 Object Lock and setting Guess MIME type and requester pays.

#### encryption

- All data transferred between the gateway and Amazon Web Services (AWS) storage is encrypted using SSL
  - Data transfers are done through HTTPS
  - objects are encrypted with:
    - Amazon S3 server-side encryption keys (SSE-S3)
    - optionally with AWS Key Management Service (AWS KMS) managed keys using SSE-KMS.

## considerations

### s3 considerations

- region: Storage Gateway is only available in select regions
- create a file share
- associate an s3 bucket
- configure storage class for new objects
- join active directory domain
- mount and use your fileshare

## integrations

### Backup

### Cloudwatch

- S3 File Gateway also publishes audit logs for SMB file share user operations to CloudWatch.
- can send a notification through CloudWatch Events when all files written to your file share up to that point in time have been uploaded
  - can then be used to activate cloud workflows after the files are uploaded, e.g. SNS notification / lambda fn

### EventBridge

- get notified when file operations are done: Storage Gateway events indicate a change
- Storage Gateway generates events that EventBridge uses.
- EventBridge receives the event and applies a rule to route the event to a target.

### cloudtrail

- track user activity
