# Backup

- centralize and automate data protection across AWS accounts, services and regions to managed S3 Buckets

## my thoughts

## links

- [landing page](https://aws.amazon.com/backup/?did=ap_card&trk=ap_card)

## best practices

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

- backup storage you use
- backup data transferred between AWS Regions
- backup data you restore
- backup evaluations

## basics

- provides a centralized backup console, a set of backup APIs, and a command line interface to manage backups across the AWS services that your applications run on
- satisfies various compliance regulations
  - FedRAMP High
  - General Data Protection Regulation (GDPR)
  - SOC 1, SOC 2, SOC 3
  - payment card industry (PCI)
  - Health Insurance Portability
  - Accountability Act of 1996 (HIPAA), and many more.

### Console

- a consolidated view of your backups and backup activity logs, making it easier to audit your backups and ensure compliance.

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

### Security

- resource-based access policies for your backup vaults
  - who has access to the backups
  - for which resources
  - what actions they can take.

## considerations

## integrations

- Backup integrates with most compute, database and storage services to centralize backup processes via policies across aws resources

### Organizations

- centrally deploy data protection policies

### Cloudtrail

### IAM

### SNS

### Storage Gateway

- use AWS Backup to back up your application data stored in AWS Storage Gateway volumes
- restore your volumes to the AWS Cloud or to your on-premises environment.

### EBS

- backing up your EBS volumes.
