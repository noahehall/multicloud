# Backup

- centralize and automate data protection across AWS accounts, services and regions to managed S3 Buckets
- removes the need to create custom scripts and manual processes.
- create backup policies called backup plans that enable you to define your backup requirements and then apply them to the AWS resources you want backed up.

## my thoughts

## links

- [landing page](https://aws.amazon.com/backup/?did=ap_card&trk=ap_card)
- [audit manager](https://docs.aws.amazon.com/aws-backup/latest/devguide/aws-backup-audit-manager.html)

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

- centrally manage backup policies that meet your backup requirements and apply them to your AWS resources across AWS services.
- define backup requirements and then apply them to the AWS resources via tags
- backup retention policies: automatically retain and expire backups
- lifecycle policies: automatically transition backups from warm storage to cold storage according to a schedule that you define.

#### Schedules

- create custom or choose from predefined backup schedules based on common best practices.
- start time
- backup frequency
  - manually
  - on-demand
  - automatically as scheduled
- backup window.

#### Retention Policies

- automatically retain and expire backups according to your business and regulatory backup compliance requirements

#### Lifecycle Management Policies

- configure lifecycle policies that will automatically transition backups from warm storage to cold storage according to a schedule that you define.

### Backup Vaults

- a container used for organizing your backups

#### Access Policies

- a central and secure way to control access to your backups across AWS services
- resource-based access policies on Backup Vaults across all users, rather than having to define permissions for each user

### Audit Manager

- set retention policies, which you can use to automatically remove old snapshots
- provides built-in compliance controls and allows you to customize those controls to define your data protection policies.
- automatically detect violations of your defined data protection policies and prompt you to take corrective actions
- continuously evaluate backup activity and generate audit reports

### Security

- resource-based access policies for your backup vaults
  - who has access to the backups
  - for which resources
  - what actions they can take.

## considerations

## integrations

- Backup integrates with most compute, database and storage services to centralize backup processes via policies across aws resources

### MGN

### EC2

### Dynamodb

### RDS

### FSx

### Organizations

- centrally deploy data protection (backup) policies to configure, manage, and govern your backup activity across your organization’s AWS accounts and resources

### Cloudtrail

### IAM

### SNS

### Storage Gateway

- use AWS Backup to back up your application data stored in AWS Storage Gateway volumes
- restore your volumes to the AWS Cloud or to your on-premises environment.

### EBS

- backing up your EBS volume snapshots to an aws managed s3 bucket
