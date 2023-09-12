# Backup

- centralize and automate data protection across AWS accounts, services and regions to managed S3 Buckets
- removes the need to create custom scripts and manual processes.
- create backup policies called backup plans that enable you to define your backup requirements and then apply them to the AWS resources you want backed up.

## my thoughts

## links

- [landing page](https://aws.amazon.com/backup/?did=ap_card&trk=ap_card)
- [AAA: Getting started](https://docs.aws.amazon.com/aws-backup/latest/devguide/getting-started.html)
- [audit manager](https://docs.aws.amazon.com/aws-backup/latest/devguide/aws-backup-audit-manager.html)
- [audit manager: blog post](https://aws.amazon.com/blogs/aws/monitor-evaluate-and-demonstrate-backup-compliance-with-aws-backup-audit-manager/)
- [ec2: backup & restore tutorial](<https://aws.amazon.com/getting-started/hands-on/amazon-ec2-backup-and-restore-using-aws-backup)
- [ebs: backup & restore tutorial](https://aws.amazon.com/getting-started/hands-on/amazon-ebs-backup-and-restore-using-aws-backup/?trk=gs_card)
- [efs: backup & restore tutorial](https://aws.amazon.com/getting-started/hands-on/amazon-efs-backup-and-restore-using-aws-backup/?trk=gs_card)
- [rds: backup & restore](https://aws.amazon.com/getting-started/hands-on/amazon-rds-backup-restore-using-aws-backup/?trk=gs_card)
- [vaults](https://docs.aws.amazon.com/aws-backup/latest/devguide/vaults.html)
- [restoring a backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-a-backup.html)
- [cross account/region copies](https://docs.aws.amazon.com/aws-backup/latest/devguide/recov-point-create-a-copy.html?icmpid=docs_console_unmapped)
- [report plans](https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-audit-reports.html)

## best practices

- Backup vs EBS Snapshots
  - backup
    - offers a more robust backup solution when compared to EBS Snapshots.
  - EBS Snapshots
    - tightly integrated with EC2 and EBS services.

### anti patterns

## features

- backup and recovery with a fully managed, policy-based service
  - Back up key data stores, such as your buckets, volumes, databases, and file systems, across AWS services.
- Centralize data protection management for your applications running in hybrid environments, such as VMware workloads and AWS Storage Gateway volumes.
- Configure, manage, and govern your backup activity across your company’s AWS accounts, resources, and AWS Regions.
  - create immutable backups to protect against accidental and malicious incidents
- monitor and prove data protection compliance by using auditor-ready reports
  - data protection policies to ensure compliance with organizational or regulatory requirements.

### pricing

- pricing is unique to each service being backed up
- storage you use
- data transferred between AWS Regions
- data you restore
- evaluations

## basics

- provides a centralized backup console, a set of backup APIs, and a command line interface to manage backups across the AWS services that your applications run on
- satisfies various compliance regulations
  - FedRAMP High
  - General Data Protection Regulation (GDPR)
  - SOC 1, SOC 2, SOC 3
  - payment card industry (PCI)
  - Health Insurance Portability
  - Accountability Act of 1996 (HIPAA), and many more.

### Copy / Restore

- copy backups either manually, as on-demand copy or automatically as part of a scheduled backup plan to multiple different regions
- cross-account backup: securely copy your backups across all AWS accounts within your AWS organizations
  - restore from the destination account or, alternatively, to the third account.

### Console

- a consolidated view of your backups and backup activity logs, making it easier to audit your backups and ensure compliance.

#### Dashboard

- audit your backup and restore activity across AWS services
- view the status of recent backup jobs and restore jobs across AWS services

### Backup Plans (Policies)

- general workflow
  - create a backup plan: start from an existing template, build a new plan, or define using JSON
  - assign applications and service resources
    - requires An IAM role with appropriate permissions
  - monitor, configure, restore, audit and report
- resource types: e.g. EBS, EC2, etc
- resource: a single resource of a resource type, e.g. an EC2 instance
- tags: Resources will only be assigned to the backup plan if they satisfy all tag conditions applied.
  - two or more tags, the effect is an AND condition.
- quotas
  - 500 Amazon Resource Names (ARNs) without wildcards
  - 30 ARNs with wildcard expressions
  - 30 conditions
  - 30 tags per resource assignment

#### rules

- specify the schedule, window, and lifecycle of a backup, and which backup vault to store backups in and which tags to add when they are created.
- rule configuration
  - backup vault
  - backup frequency
  - backup window
  - transition to cold storage
  - retention period
  - cross-region copy
  - cross-account copy
  - tags added to recovery points
  - advanced settings
    - Backup and restore your Volume Shadow Copy Service (VSS) enabled Microsoft Windows applications

#### Schedules

- create custom or choose from predefined backup schedules based on common best practices.
  - start time
  - backup frequency: include Hourly, Every 12 hours, Daily, Weekly, Monthly, or Custom cron expression.
    - manually
    - on-demand
    - automatically as scheduled
  - backup window: start time + duration; Backup jobs are started within this window.
    - default backup window is set to start at 5 AM UTC and lasts 8 hours.
- recovery point: represents the content of a resource at a specified time
  - include metadata such as information about the resource, restore parameters, and tags.

#### Retention Policies

- how long to store your backups.
- automatically deletes your backups at the end of this period to save on storage costs
- can be retained between 1 day and 100 years (or indefinitely, if you do not enter a retention period),
  - continuous backups between 1 and 35 days.

#### Lifecycle Management Policies

- Resource types, such as Amazon EFS and DynamoDB, can be transitioned to cold storage automatically.
  - When not applicable, the setting is ignored.
- recovery points that transitions to cold storage must remain there for at least 90 days.
- retention period setting must be at least 90 days after your transition to cold storage setting.

### Backup Vaults

- a container used for organizing your backups
  - maintains immutable copies of your backups, preventing any user from altering or deleting your backup data.
  - enforces a write-once, read-many (WORM) setting for all the backups
- Use multiple vaults to store, organize, and apply different access policies across your backups
  - Separate permissions; for example, development, test, and production
  - unique SNS notification configuration per vault.
- create up to 100 backup vaults per AWS Region.
- vault names are unique & case sensitive
  - contain 2–50 alphanumeric characters, hyphens, or underscores.

#### Access Policies

- a central and secure way to control access to your backups across AWS services
- resource-based access policies on Backup Vaults across all users, rather than having to define permissions for each user

#### Vault Lock

- fortify compliance requirements by protecting your backups and lifecycles against intentional or accidental actions, such as deletions
- uses a WORM model to make the data immutable
  - no users—including root, administrators, or bad actors—can delete your backups or change their lifecycle settings such as retention periods and transition to cold storage
- keeps these backups according to your scheduled retention periods.

### Audit Manager

- audit the compliance of AWS Backup policies against controls (schedules, retention policies, etc)
- set retention policies, which you can use to automatically remove old snapshots
- automatically detect violations of your defined data protection policies and prompt you to take corrective actions
- continuously evaluate backup activity and generate audit reports
- control: a procedure designed to audit the adherence of a backup requirement, such as the backup frequency or the backup retention period
  - resources protected by backup plan: identify gaps in your backup coverage.
  - minimum frequency and retention: governing how frequently the backup plan should be taking backups and for how long recovery points should be maintained.
  - prevent recovery point manual deletion: add up to five IAM roles allowed to manually delete recovery points if there are exceptions.
  - recovery point encryption: evaluates if the backup recovery points are encrypted
  - recovery point minimum retention: ensuring that resources have valid recovery points and are retained for at least the specified backup recovery point minimum retention period.
- framework: a collection of controls that can be managed as a single entity
  - create a freamwork for each regulatory standard/process you must comply with

#### Report plans

- visibility into your backup activities, where you can monitor your operations, identify failures, and take proactive steps to resolve or improve your setup.
- delivers a daily report in CSV, JSON, or both formats to your Amazon S3 bucket.
  - can also run an on-demand report anytime.
  - s3 bucket must already exist
- maximum of 20 report plans per AWS account.
- templates: defines the information you want included in your report.

### Events

- supports these events: backup job, copy job, and restore job, and recovery point jobs.

### Security

- resource-based access policies for your backup vaults
  - who has access to the backups
  - for which resources
  - what actions they can take.

#### Encryption

- By default, AWS Backup creates an AWS KMS key with the alias aws/backup
  - use this or choose a different one
  - After you create a backup vault and set the AWS KMS encryption key, you can no longer edit the key for that backup vault.

## considerations

## integrations

- Backup integrates with most compute, database and storage services to centralize backup processes via policies across aws resources
- Continuous backup for PITRs are available for RDS and S3 resources
  - The most time that can elapse between the current state of your workload and your most recent PITR is 5 minutes.
  - can be stored for up to 35 days.

### MGN

### EC2

- Backing up and restoring an EC2 instance
  - root Amazon EBS storage volume
  - All associated EBS volumes
  - Launch configurations
    - Instance type
    - security groups
    - VPC
    - monitoring configuration
    - tags
  - Amazon Machine Image (AMI), including all launch configurations
- The backup data for compute is stored as an EBS volume-backed AMI and pushed to an s3 bucket
- whats not backed up
  - Configuration of the Elastic Inference accelerator, if it is attached to the instance
  - User data used when the instance was launched

### Dynamodb

- turn on the AWS Backup advanced features in your AWS Region
  - Tiering backups to cold storage to reduce storage costs
  - Cost allocation tagging for use with AWS Cost Explorer
  - Cross-Region copy
  - cross-account copy
  - Store backups in encrypted backup vaults, which you can secure with AWS Backup Vault Lock, backup policies, and encryption keys.
  - Backups inherit tags from their source DynamoDB tables, so you can use those tags to set permissions and service control policies (SCPs).

### DocumentDB

- supports a single data protection policy in AWS Backup to automate the
  - creation of independent, immutable, and protected snapshots of Amazon DocumentDB clusters across AWS Regions or accounts
  - restore your clusters from these snapshots

### RDS

- creates a storage volume snapshot of your DB instance, backing up the entire DB instance and not just individual databases
- saves the automated backups of your DB instance according to the backup retention period that you specify
- recover your database to any point in time during the backup retention period.
  - Scheduled snapshots
  - On-demand snapshots
  - Continuous PITR snapshots

### Aurora

- manage Aurora database cluster snapshots
  - creation, management, and restoration of Aurora backups directly from the AWS Backup console for both PostgreSQL-compatible and MySQL-compatible versions of Aurora
- centrally configure backup policies, monitor backup activity, and copy a snapshot within and across AWS Regions.

### Neptune

- Supports continuous and incremental Neptune backups
- you specify a backup retention period, from 1 to 35 days, when you create or modify a DB cluster

### FSx

- Windows File Serve: file-system-consistent, highly durable, and incremental.
- Lustre: automatic daily backups and user-initiated backups of persistent file systems that are not linked to an Amazon S3 durable data repository.

### IAM

- unified IAM policy control over which accounts and roles can access backup resources

### Organizations

- organization-wide backup protection and monitoring
- manage and monitor all of your backups from a single management account
- deploy data protection (backup) policies to configure, manage, and govern backup activity
- trusted access
  - Use the AWS Backup console to view details about the backup, restore, and copy jobs in any of the accounts in your organization

### cloudwatch

- monitor AWS Backup metrics by using the aws/backup namespace.

### Cloudtrail

### IAM

### SNS

### Storage Gateway

- use AWS Backup to back up your application data stored in AWS Storage Gateway volumes
- restore your volumes to the AWS Cloud or to your on-premises environment.
- Supports backup and restore of both cached and stored volumes

### EBS

- uses the native EBS snapshot APIs for making backups (to an s3 buket)
- create a snapshot and restore that snapshot to a new volume in the SAME region
- copy snapshots to other Regions and then restore them to new volumes there

### EFS

- incrementally backup all data in an EFS file system, whatever storage class the data is in.
  - Tiering backups to cold storage to reduce storage costs
  - Cost allocation tagging for use with AWS Cost Explorer
  - backups can also be transitioned to lower tier storage, which helps in optimizing costs.
- restore the entire file system or restore specific individual files and directories.

### S3

- create a backup policy and assign S3 buckets to it with tags or resource IDs.
  - You must activate S3 Versioning on your bucket before AWS Backup can back it up.
  - recommends that you set a lifecycle expiration rule for versioning-enabled buckets
    - else storage costs might increase because AWS Backup will retain all versions of your Amazon S3 data.
- Create nearly continuous point-in-time and periodic backups.
- Automate backup scheduling and retention by centrally configuring backup policies.
- Restore backups of Amazon S3 data to a specific point in time.

### VMware

- centrally protect your on-premises VMware and VMware Cloud in AWS environments.
- built-in controls for VMware backups so you can track backup and restore operations and generate auditor-ready reports.
- a single-click restore experience so you can restore VMware backups on-premises and in VMware Cloud on AWS.

### SNS

- configure Amazon SNS to notify you of AWS Backup events from the Amazon SNS console.

### EventBridge

- monitor and log AWS Backup events to help support your regulatory compliance reporting obligations and meet your business continuity SLAs.
