# Compute Optimizer

- recommendations to optimize use of:
  - EC2 instance types and autoscaling groups
  - EBS volumes
  - ECS on AWS Fargate
  - Lambda functions—based on your utilization data.

## my thoughts

## links

- [landing page](https://aws.amazon.com/compute-optimizer/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

- Rightsize workloads with artificial intelligence and machine learning-based analytics to reduce costs by up to 25%.
- Resolve performance issues by implementing recommendations that identify underprovisioned resources.
- Increase recommendation savings and visibility into memory utilization by enabling Amazon CloudWatch metrics.
- Configure enhanced infrastructure metrics
- Find the EC2 workloads that will deliver the biggest return for the smallest migration effort in a shift to AWS Graviton CPUs.

### pricing

- The default Compute Optimizer option analyzes your Amazon CloudWatch metrics for the last 14 days to provide recommendations.
  - i think this is the free version since you're already paying for the running services
- EC2 or Auto Scaling group recommendations that capture monthly or quarterly utilization patterns
  - activate Compute Optimizer enhanced infrastructure metrics
    - this is the paid version
    - analyzes up to six times more Amazon CloudWatch utilization metrics history than the default
    - cost $0.0003360215 per resource per hour and are charged based on the number of hours per month the resource is running.
      - e.g. $0.25 per resource per month for resources running a full 31-day month.

## basics

## considerations

## integrations
