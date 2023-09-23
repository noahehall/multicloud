# elastic load balancing (ELB)

- distribute network traffic across EC2, containers, ip address and lambda functions
- its in the ec2 console
- [ALB](./elb-alb.md)
- [NLB](./elb-nlb.md)
- [GLB](./elb-glb.md)
- classic load balancer is kept in this file; shouldnt be used

## my thoughts

- why tf is everything in the ec2 console

## links

- [landing page](https://aws.amazon.com/elasticloadbalancing/?did=ap_card&trk=ap_card)
- [user guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)
- [type comparisons](https://aws.amazon.com/elasticloadbalancing/features/#Product_comparisons)
- [blue green deployments](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-create-loadbalancer-bluegreen.html)

## best practices

- ELB basic comparison: check the comparison link for indepth
  - ALB: layer 7; ip, instance and lambdas
    - preverse source ip addr
    - fixed response
    - user authnz
  - NLB: layer 4; ip, instance and ALBs
    - preserve source ip addr
    - static ip and elastic ip addr
  - GLB: ip and instances
    - preserve source ip addr

### anti patterns

## features

- secure apps with integrated certificate management, authnz and ssl/tls decryption
- monitor health and perf metrics in real time
- highly available and automatically scalable frontdoor: its not a single point of failure

### pricing

- charged by the hour based on load balancer capacity units

## terms

## basics

- its a regional service, AWS manages provisioning and scaling across regions to meet demand
- all elb types uses the same API, i.e. target groups, health checks, etc

### OSI Model

- manages layer 4 and layer 7, but works at layer 7
  - layer 4: network load balancer; does not understand/read the network packets
  - layer 7: application load balancer; inspects packets, has access to http/s headers, and can intelligently load balance traffic

### listeners

- clientside mechnism
- checks for requests on a specific port and protocol
- when multiple listeners exist, they are checked in priority order

### target groups

- server side mechanism
- health checks
  - healthy threshold: consecutive successful health checks before considering an unhealthy target to be healthy
  - unhealthy threshold: consecutive unhealhthy health checks before considering a healthy target to be unhealthy
  - timeout: amount of time to wait to receive a response before determining a health check has failed
  - interval: amount of time between health checks
  - success codes: generally something like 200, or 200-299
- the type of backend your directing traffic to, e.g. lambdas, ip addrs, ec2 intance, other load balancers, etc
  - each specific backend resource within a target group requires a health check

### rules

- defines how requests are routed to targets in a target group
- requires two conditions
  - source IP: when the source ip matches, the listener will handle the request
  - target group: where to matched requets

### Classic Load Balancer

- the first load balancer type
- not recommended for use unless you have legacy services or applications that need the Classic Load Balancer.

## considerations

- scheme: either internet-facing oor internal
- ip addr type: ipv4 or dualstack (ip4+6)
- networking mapping: vpc, AZs and subnets
- security groups: see securitygroups file
- target group: depends on the type of target group youve selected

## integrations
