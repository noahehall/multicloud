# elastic file system (EFS)

- serverless, elastic file system for any AWS compute service

## my thoughts

## links

- [landing page](https://aws.amazon.com/efs/?did=ap_card&trk=ap_card)
- [total cost of ownership example](https://aws.amazon.com/efs/tco/)

## best practices

### anti patterns

## features

- persistent storage for content management systems, machine learning and big data analytics workloads
- create and configure shared file systems for AWS compute services
- eliminate capacity planning
- 11 9s durability and up to 49s of availability
- automate deletion/movement of infrequently accessed files

### pricing

- pay for total data stored
- read and write access to data
  - stored in Infrequent Access storage classes
  - using Elastic Throughput
  - using Provisioned Throughput

## terms

## basics

## considerations

- storage class
  - multi-az; highest levels of durability and availabilityt
    - standard:
    - standard IA: infrequent access
  - single AZ; cheapest
    - one zone:
    - one zone IA: infrequent access

## integrations

### EKS

- integrates with k8s via an EFS CSI driver
- use cases
  - application workloads requiring replicas to span across worker nodes and access the same application data
- general workflow
  - a k8s storage class backed by EFS will direct the EFS CSI driver to make calls to the appropriate aws apis to create an access point to a preexisting file system
  - when a peristent volume claim (PVC) is created
  - a dynamically provisioned peristent volume (PV) will use the access point for access to EFS file system then bind to the PVC
