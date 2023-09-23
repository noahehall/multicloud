# Network Load Balancer (NLB)

- Load balance TCP/UDP/TLS traffic with high performance.
- capable of handling millions of requests per second while maintaining ultra-low latencies
- layer 4 tcp/udp/tls

## my thoughts

## links

- [landing page](https://aws.amazon.com/elasticloadbalancing/network-load-balancer/?nc=sn&loc=2&dn=3)

## best practices

### anti patterns

## features

- source IP preservation
- low latency
- sticky sessions
- optimized to handle sudden and volatile traffic patterns while using a single static IP address per AZ
- automatically receives a static IP addr per AZ subnet
- can assign a custom fixed elastic ip addr per AZ subnet
- dns failover via route 53 to direct traffic to load balancer nodes in other zones
- can be enabled in a single AZ to support architectures that require zonal isolation.

### pricing

## basics

- enable access to resources within a private vpc

### Protocols

#### TCP

- supports long-lived TCP connections that are ideal for WebSocket type of applications.

### Targets

- EC2 instances, microservices, and containers within Amazon VPC, based on IP protocol data.

### monitoring

### security

## considerations

## integrations

### EKS

- managed by LoadBalancer services
  - used with pods deployed to EC2 IP and instance targets or farget IP targets

### Auto Scaling

### ECS

### Cloudformation

### ACM

### Route 53

- DNS Fail-over: will direct traffic to load balancer nodes in other Availability Zones.
  - no healthy targets registered
  - NLB nodes in a given zone are unhealthy

### Elastic Beanstalk

### Cloudtrail

### Cloudwatch

### Config

### ACM

### CodeDeploy
