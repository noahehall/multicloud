# Elastic Disaster Recovery (DRS)

- Scalable, cost-effective application recovery to AWS EBS
- [fka CloudEndure](https://www.cloudendure.com/)

## my thoughts

## links

- [landing page](https://aws.amazon.com/disaster-recovery/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

- removing idle recovery site resources, and pay for your full disaster recovery site only when needed.
- Recover your applications within minutes, at their most up-to-date state or from a previous point in time.
  - replicates entire machines, including OS, system state configuration, system disks, databases, applications, and files
- unified process to test, recover, and fail back a wide range of applications, without specialized skillsets.
  - triggers a highly automated machine conversion process and a scalable orchestration engine
- add or remove replicating servers as needed.
- On-premises to AWS; Cloud to AWS; AWS Region to AWS Region
- provides continuous, asynchronous, block-level replication of your source machines into a staging area

### pricing

- billed hourly per source server registered, irrespective of provisioned storage capacity
  - continuous data replication
  - virtually unlimited disaster recovery drills
  - point-in-time recovery
  - automated failover and failback.
- staging resources that CloudEndure Disaster Recovery creates
  - EC2/EBS during continuous replication.
- Payment for fully provisioned resources is required only during
  - disaster recovery drills
  - recovery mode.

## basics

- uses the same replication method as MGN to stage copies of your on-premises applications to EC2 instances and EBS storage
- in the event of a failover requirement, DRS resources are elevated to AWS resources capable of handling the full application workload.

## considerations

## integrations
