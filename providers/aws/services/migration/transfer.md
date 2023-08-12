# Tranfser

- manage and share data with simple, secure, and scalable file transfers

## my thoughts

## links

- [landing page](https://aws.amazon.com/aws-transfer-family/?c=s&sec=srv)

## best practices

### anti patterns

## features

- manage & migrate file transfer workflows to S3/EFS and integrate existing authentication systems, and providing DNS routing with Route 53
- Store information in S3 or EFS, manage workflows, and trigger automated, event-driven tasks
- Support thousands of concurrent users with access controls
- security requirements with data encryption, VPC and FIPS endpoints, compliance certifications, PGP, and more.
- your existing SFTP/FTPS/FTP applications and clients do not change, only the storage endpoint to Amazon S3 or Amazon EFS.

### pricing

- the protocols enabled on your Server endpoint(s)
- the amount of data (gigabytes) uploaded and downloaded over SFTP/FTPS/FTP
  - using SFTP connectors to connect to remote SFTP servers:
    - the number of connector calls
    - the amount of data sent or retrieved.
- the number of messages sent or received over AS2
- the amount of data processed using AWS Transfer Family Workflows

## basics

- general workflow
  - select one/more protocols
  - configure s3 buckets or EFS file systems
  - setup authnz
    - import your existing user credentials
    - integrate an identity provider such as Microsoft ADor LDAP.

### architecture

- external clients: connect over the public internet and access data via SFTP/FTPS
- internal clients: connect over the public (VPN/PrivateLink) or private (DirectConnect) using SFTP/FTPS or FTP (since its a private connection )
- VPC: connected clients access resources in AWS via an Internet/Private gateway
- setup authnz
  - IAM
  - Microsoft AD
  - LDAP
- setup Storage services
  - S3
  - EFS

### Servers

- AWS transparently operates and manages all the compute, storage, and other infrastructure necessary to maintain high availability and performance for your server endpoints
  - servers autoscale to match demand
  - redundancy across multiple Availability Zones within an AWS Region.
- endpoint provides a URL that provides direct logical connectivity to a service in an AWS Region

#### SFTP Enabled

#### FTPS Enabled

#### FTP enabled

### Endpoints for file sharing

- supported protocols: SFTP, FTPS, FTP

#### SFTP Connectors

- copy data between remote SFTP servers and S3

### Applicability Statement 2 (AS2 protocol)

- transfer files between partners, vendors, and customers into and out of S3 using the AS2 protocol.

### Managed Workflows

## considerations

## integrations

- data in S3 and EFS preserves relevant file metadata
- once data is AWS you can integrate as normal

### Route 53

### S3

### EFS

### Cloudwatch

### IAM

### KMS

### Cloudtrail
