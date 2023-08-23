# service name

- Batch processing, ML model training, and analysis at any scale
- specify the basic resource parameters such as GPU, CPU, and memory requirements, and AWS Batch does the rest.

## my thoughts

## links

- [landing page](https://aws.amazon.com/batch/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

- Run hundreds of thousands of batch and machine learning (ML) computing jobs without installing software or servers.
- optimizing computing job distribution based on volume and resource requirements.
- fully managed infrastructure that supports large-scale processing and simulations.
- dynamically provisions the optimal quantity and type of compute resources, such as CPU- or memory-optimized compute resources,
- plans, schedules, and runs your batch computing workloads using Amazon EC2 and AWS compute resources with Fargate or Fargate Spot.

### pricing

- pay for AWS resources you create to store and run your application.
  - EC2 instances: Reserved Instances, Savings Plan, EC2 Spot Instances
  - AWS Lambda functions
  - AWS Fargate: Fargate or Fargate Spot

## basics

### EC2

- For additional control and scale, use EC2 or EC2 spot instances, which Batch provisions and manages for you.

### Fargate

- every job receives the exact amount of CPU and memory that it requests

## considerations

## integrations
