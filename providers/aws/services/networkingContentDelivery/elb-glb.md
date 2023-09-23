# Gateway Load Balancer (GLB)

- deploy, scale, and manage your third-party virtual appliances
- layer 3 gateway and layer 4 loadbalancer

## my thoughts

## links

- [landing page](https://aws.amazon.com/elasticloadbalancing/gateway-load-balancer/#Features)
- [intro](https://aws.amazon.com/blogs/aws/introducing-aws-gateway-load-balancer-easy-deployment-scalability-and-high-availability-for-partner-appliances/)

## best practices

### anti patterns

## features

- find, test, and buy virtual appliances from third-party vendors directly in AWS Marketplace.
- deploy scale and manage thirdparty appliances like firewalls, intrusion detection adn prevension systems, and deep packet inspection systems
- It listens for all IP packets across all ports and forwards traffic to the target group that's specified in the listener rule.
- It maintains stickiness of flows to a specific target appliance using 5-tuple (for TCP/UDP flows) or 3-tuple (for non-TCP/UDP flows).
  - high availabilty routing to backends via health checks
  - gateway load balancer endpoints
  - private connectivity to internet gateways, VPCs and other resources over a private network

### pricing

## basics

- mainly used to load balance requests to third party applications
- uses Gateway Load Balancer endpoints to securely exchange traffic across VPC boundaries.
- Gateway Load Balancer endpoint is a VPC endpoint that provides private connectivity between virtual appliances across VPCs

### Gateway Load Balancer Endpoints

- a new type of VPC endpoint. Powered by PrivateLink
- traffic flows over the AWS network, and data is never exposed to the internet.

### monitoring

### security

## considerations

## integrations

### Cloudwatch
