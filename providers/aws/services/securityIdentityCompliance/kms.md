# key management service (KMS)

- centrally manage creation and control of encryption keys
- tightly integrated with managed services for encryption at rest and inflight

## my thoughts

## links

- [best practices pdf](https://d0.awsstatic.com/whitepapers/aws-kms-best-practices.pdf)
- [concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)
- [cryptographic details pdf](https://d0.awsstatic.com/whitepapers/KMS-Cryptographic-Details.pdf)
- [faqs](https://aws.amazon.com/kms/faqs/?da=sec&sec=prep)
- [landing page](https://aws.amazon.com/kms/?did=ap_card&trk=ap_card)
- [overview](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
- [protecting SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-server-side-encryption.html)

## best practices

- for most services
  - enable Encryption by default: you can use Managed/Customer keys

### anti patterns

## features

- centrally manage keys and define policies across integrated services and applications
- encrypt data locally within applications using an sdk
- sign and validate diginal signatures using asymmetric key pairs
- generate HMACs that ensure message integrity and authenticity
- control access to your encrypted data by defining the permissions to use the keys while AWS KMS enforces your permissions and handles the durability and physical security of your keys.
  - a single control point to manage keys and define policies consistently across integrated AWS services and your own applications.
-

### pricing

- each CMK costs $1/month

## basics

- You submit data to AWS KMS to be encrypted or decrypted
  - the keys never leave KMS
  - set usage policies on these keys that determine which users can use them
- aws vs customer managed keys
  - aws managed
    - creation: AWS
    - rotation: automatically once every 3 years
    - deletion: cant be deleted
    - scope of use: a key generated for each specific service
    - key access policy: AWS managed
    - user access management: IAM policy
  - customer managed
    - creation: you
    - rotation: opt-in to once a year or manually
    - deletion: manually
    - scope of use: controlled via KMS / IAM policy
    - key access policy: you decide
    - user access management: IAM policy
-

### key types

- each key receives an Amazon Resource Name (ARN) that includes a unique key identifier, or key ID

#### AWS Owned

- created and exclusively used by AWS for internal encryption operations across different AWS services
- Customers do not have visibility into key policies or AWS owned key usage in CloudTrail.

#### AWS Managed

- the default option for services when integrating with KMS
- AWS creates and controls the lifecycle and key policies of AWS managed keys

#### Customer Master Keys CMK

- Customers create and control the lifecycle and key policies of customer managed keys.
- benefits of CMKs
  - You manage the rotation of the keys
  - Easier to manage a few primary keys as opposed to billions of data keys
  - Centralized access and auditing
  - Performs better for large datasets

### Data keys

- Cryptographic keys generated on hardware security modules (HSMs) protected by a KMS key.-
- authorized entities can obtain data keys protected by a KMS key
- can be symmetric or asymmetric, with both the public and private portions returned.

### Grants

- The delegated permission to use a KMS key when the intended IAM principals or duration of use is not known at the outset, and therefore cannot be added to a key or IAM policy.
- use cases
  - deﬁne scoped-down permissions for how an AWS service can use a KMS key.

### algorithms

- check the security file for a deep dive
- symmetric: same key is used for encryption and decryption
  - never leaves the AWS KMS service unencrypted.
- assymetric: public key and private key pair that you can use for encryption and decryption or signing and verification, but not both
  - private key never leaves the KMS service unencrypted
  - use the public key within AWS KMS by calling the AWS KMS API operations or downloading it
- envelope encryption: the practice of encrypting plaintext data with a data key and then encrypting the data key under another key.

## considerations

- key types
  - symmetric
  - assymetric
  - HMAC
  - multi-region (primary and replica)
  - keys with imported key material

## integrations

- generally all AWS services can utilize KMS for encryption whether at rest or in transit

### cloudtrail

- All requests to use any KMS key is logged in cloudtrail
- encrypts all log files delivered to your specified Amazon S3 bucket using Amazon S3 server-side encryption (SSE).
- Optionally, add a layer of security by encrypting the log files with AWS KMS keys
  - S3 automatically decrypts your log files if you have decrypt permissions.

### s3

- KMS generates a unique key to encrypt each object transparently to the user
  - the data key is encrypted under a master key provided by aws/user
- default encryption
  - uses SSE-s3 or SSE-kms
- custom encryption: when the object PUT request header contains a different encryption method
  - instead of using the buckets default encryption, it will use whatevers in the header
- bucket keys for SSE-KMs: configure your bucket to use an S3 Bucket Key for SSE-KMS
  - KMS generates a bucket-level key that is used to create unique data keys for new objects that you add to the bucket
    - instead of making an api request from s3 to KMS each time

#### at rest

- three mutually exclusive server side encryption options, depending on how you choose to manage the encryption keys.
- S3-managed Keys: SSE-S3; the base level of encryption for every bucket and all new objects
  - each object encrypts with a unique key
  - encrypts the key itself with a master key that it regularly rotates
  - SS3-S3 keys are managed within S3, but i think it all eventually uses kms
- KMS keys: SSE-KMS; uses customer Master Keys (CMKs) Stored in KMS
  - additional benefits and charges for using this service compared with SSE-S3
  - separate permissions for the use of a CMK that provides added protection against unauthorized access of your objects in Amazon S3
  - an audit trail showing when and who used the CMK
  - create and manage customer managed CMKs, or use AWS managed CMKs that are unique to you, your service, and your Region.
- Customer-provided keys: SSE-C;
  - you manage the encryption keys
  - S3 manages the encryption before writing to disk, and decryption on object access

#### in transit

- either HTTPS or client-side encryption.
- use an S3 bucket policy to force the use of HTTPS requests.

### EBS

- supports only symmetric KMS keys.
- The encryption status of an EBS volume is determined when you create the volume.
  - migrate data between encrypted and unencrypted volumes and apply a new encryption status while copying a snapshot.
- encryption can be enabled by default at the account level
  - any new volumes will be automatically encrypted
  - The same data key is shared by snapshots of the volume and any subsequent volumes created from those snapshots.
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
  - cannot change the KMS key that is associated with an existing snapshot or volume
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
- workflow
  - For EBS to encrypt a volume for you, it must have access to generate a volume key (VK) under a KMS key in the account.
  - do this by providing a grant for Amazon EBS to the KMS key to create data keys and to encrypt and decrypt these volume keys.
- gotchas
  - encryption by default
    - is a region-specific setting and applied to all ebs volumes in that region
    - forces use of instance types that supports EBS encryption.
    - copy a snapshot and encrypt it to a new KMS key, a complete, non-incremental copy is created (more storage costs)
  - configuring a KMS key as the default key for EBS encryption
    - the default KMS key policy allows any IAM user with access to the required KMS actions to use this default key to encrypt or decrypt EBS resources.
    - search the docs for the required KMS permissions

### neptune

- On an encrypted Neptune instance, data in the underlying storage is encrypted, as are the automated backups, snapshots, and replicas in the same cluster.
