# route53

- anycast DNS for AWS

## my thoughts

## links

- [landing page](https://aws.amazon.com/route53/?did%253Dap_card%2526trk%253Dap_card)
- [hosted zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-working-with.html)
- [record types](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/rrsets-working-with.html)
- [routing traffic](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-routing-traffic-to-resources.html)
- [configuring DNS](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html)
- [registrar](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar.html)
- [blog: global fault isolation](https://aws.amazon.com/blogs/architecture/a-case-study-in-global-fault-isolation/)

## best practices

### anti patterns

## features

- route end users with globally-dispered DNS servers and automatic scaling and visual traffic flow tools
- customizable dns routing policies to reduce latency, improve application avialability and maintain compliance
- health checks for resources to ensure availability and notifications when they go offline

### pricing

- pay for managing domains and queries the service answersd
- monthly charge for each hosted zone
  - .50 / month for the first 25
  - .10 / month for each additional
- cost per dns query answered by route53
  - these requests are free: alias A mapped to elastic load balancing instances, cloudfront distros, beanstalk, api gateway, vpc endpoints, s3 buckets
- managed domain names: charged annually

## basics

### OSI Model

- operates at layer 7: an application built on top of the TCP/IP stack toconvert hostnames to an ip addr
  - transports to query DNS servers for name resolution using TCP/UDP
  - it sends back the correct ip addr
  - uses the TCP/IP stack to pass the destination IP addr found by DNS to the transport layer > network layer > and so forth

### Domain Registration

- transfer, search and purchase domain names

### Domain Hosting

- managed DNS hosting service: you create hosted zones for domains

#### public hosted zone

- how you want to route traffic on the internet
- each hosted zone gets four name servers

#### private hosted zone:

- how you want to route traffic within a VPC

### health checks

- monitor resources ensuring responses only point to healthy resources

### resolver

- DNS resolution within a VPC without using custom DNS servers
- response to DNS queries for
  - internet routable domain names
  - private hosted zone domains

### Routing policies

#### Weighted

- route traffic to resources based on specified ratios

#### geo location

- route traffic based on user location

## considerations

## integrations

### IAM

### ecs

- autonaming
  - manually create a namespace in a hosted zone
    - then ECS should handle the rest automagically
      - CNAMEW record per service autoname
      - A records per task IP
      - SRV records per task IP + port
