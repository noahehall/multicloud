# Application Migration Service (MGN)

- migrate applications to AWS, GovCloud (US), and Outposts.
- highly automated lift-and-shift (rehost) solution
- [fka CloudEndure](https://www.cloudendure.com/)

## my thoughts

## links

- [landing page](https://aws.amazon.com/application-migration-service/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

- lift-and-shift physical, virtual, or cloud servers without compatibility issues, performance impact, or long cutover windows
  - supports migrations from VMware vSphere, Microsoft Hyper-V, Amazon EC2, and other clouds to AWS.
- Migrate applications from any source infrastructure that runs supported operating systems.
- continuously replicates your source servers to your AWS account
- automatically converts and launches your servers on AWS.
- Migrate EC2 workloads across AWS Regions, AZs, or accounts

### pricing

- use AWS MGN for a free period of 2,160 hours, which is 90 days when used continuously.
- free period starts as soon as you install the AWS Replication Agent on your source server and continues during active source server replication.
  - if not completed within the free period
    - charged per hour while you continue replicating that server.

## basics

- general workflow
  - installing the AWS Replication Agent on your source servers.
  - view and define replication settings
  - MGN uses these settings to create and manage a Staging Area Subnet with resources
    - lightweight EC2 instances that act as replication servers
    - low-cost staging Amazon EBS volumes.
  - Replication Servers receive data from the agent running on your source servers
    - write this data to the staging Amazon EBS volumes.
    - source servers on AWS by use continuous, block-level data replication.
  - launch Test or Cutover instances
    - converts your source servers to boot and run natively on AWS automatically.
    - if tests pass: you can decommission your source servers.

### Security

#### Encryption

- replicated data is compressed and encrypted in transit
- can be encrypted at rest using Amazon EBS encryption.

## considerations

## integrations

### DRS Elastic Disaster Recovery

- replicate and recover from any of the following:
  - On-premises to Outposts
  - From AWS Regions onto Outposts
  - From Outposts into AWS Regions
  - Between two Outposts.

### CloudEndure Migration

- can be used instead of MGN for
  - unsupported AWS regions
  - unsupported operating systems
