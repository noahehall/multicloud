# Macie

- uses machine learning to automatically discover, classify, and protect sensitive data like PII/IP
- monitors data usage, PII, and IP, inventory of S3 buckets and their access control and encryption settings
- provides you with dashboards and alerts that give visibility into how this data is being accessed or moved

## my thoughts

- seems to be entirely focused on data stored in s3
- yea its an s3 addon service

## links

- [landing page](https://aws.amazon.com/macie/?did=ap_card&trk=ap_card)
- [custom alerts](https://aws.amazon.com/blogs/security/how-to-create-custom-alerts-with-amazon-macie/)
- [classifying sensitive data](https://aws.amazon.com/blogs/security/classify-sensitive-data-in-your-environment-using-amazon-macie/)
- [data classification](https://d1.awsstatic.com/whitepapers/compliance/AWS_Data_Classification.pdf)

## best practices

### anti patterns

## features

- automate sensitive data discovery at scale
  - identify data with high business value, including programming languages, to detect source code, logging formats, database backup formats, credentials, and API key formats.
- assess S3 bucket inventory for security and access controls
- user behavior analytics engine of Macie helps identify risky or suspicious activity with AWS service API calls and access to high-value content
- Schedule data analysis to certify that sensitive data is discovered and protected.
- During data ingestion, determine if sensitive data has been appropriately protected
- integrate with SIEM services and managed security service provider (MSSP) solutions.

### pricing

- the number of s3 buckets evaluated for bucket inventory and monitoring
- the number of S3 objects monitored for automated data discovery
- the quantity of data inspected for automated and targeted sensitive data discovery.

## basics

- scans s3 buckets for
  - bucket and object resources
  - configuration and security attributes
  - Read, write, and delete actions on Amazon S3
  - IAM users, roles, and access policies associated with the S3 resources
  - buckets and objects with certain keywords
  - objects containing certain type of data

## considerations

## integrations

### CloudTrail

### Cloudwatch

- discovering relevant data fields collected by Macie and turning those fields into custom alerts.

### eventbridge

- As security findings are generated, they are pushed out to Amazon EventBridge

### GuardDuty

- integrate with both CloudTrail and Amazon GuardDuty to help monitor the permission and security of the buckets
- CloudTrail data about access and permissions is limited to account-level and bucket-level settings and doesn’t reflect object-level settings
