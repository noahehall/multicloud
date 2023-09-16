# route53

- anycast DNS for AWS

## my thoughts

## links

- [blog: global fault isolation](https://aws.amazon.com/blogs/architecture/a-case-study-in-global-fault-isolation/)
- [dns: alias vs non-alias records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)
- [dns: configuratoin](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html)
- [dns: hosted zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-working-with.html)
- [dns: migrating existing domains](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/MigratingDNS.html)
- [dns: records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/rrsets-working-with.html)
- [dns: routing traffic](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-routing-traffic-to-resources.html)
- [landing page](https://aws.amazon.com/route53/?did%253Dap_card%2526trk%253Dap_card)
- [registrar](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar.html)
- [dns: routing policies](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html)
- [blog: split-view DNS](https://repost.aws/knowledge-center/internal-version-website)

### tools

- [dig: nix command](https://phoenixnap.com/kb/linux-dig-command-examples)

## best practices

### anti patterns

- does not support zone transfers
- no concept of primary/secondary name servers
- DNSSEC is supported for domain registration; but NOT for DNS
  - use another DNS service provider to configure DNSSEC for a domain registered with r53
- no support for BIND views
  - but does support split-view DNS

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
  - free:
    - alias records mapped to elastic load balancing instances, cloudfront distros, beanstalk, api gateway, vpc endpoints, s3 buckets
- managed domain names: charged annually

## basics

### OSI Model

- operates at layer 7: an application built on top of the TCP/IP stack toconvert hostnames to an ip addr
  - transports to query DNS servers for name resolution using TCP/UDP
  - it sends back the correct ip addr
  - uses the TCP/IP stack to pass the destination IP addr found by DNS to the transport layer > network layer > and so forth

### registrar: Domain Registration

- transfer, search and purchase domain names

#### DNSSEC

- domain name system security extensions

### Domain Hosting

- managed DNS hosting service: you create hosted zones for domains

#### hosted zones

- zone apex: the domain the hosted zone is for
- changes in hosted zones are propagated to all name servers simultaneously

##### public hosted zone

- how you want to route traffic on the internet
- each hosted zone gets four name servers

##### private hosted zone

- how you want to route traffic within one/more VPCs
- only an r53 resolver can resolve records in a private hosted zone
- VPC Settings required for private hosted zones
  - enableDnsHostnames
  - enableDnsSupport
- Health checks
- Routing policies
- split-view DNS
- namspaces
- delegating subdomain responsibility

##### Split-view DNS

##### Records

###### Alias

- r53 specific DNS extension for mapping domain names
- considerations
  - type: alias records must have the same type as the target record being routed to
  - zone apex: can create records for the zone apex
  - values: where traffic should be routed
    - supported services:
    - other records of the same type within the same hosted zone
  - change detection: r53 will automatically route to the underlying resource, e.g. changes to an ELB ip is automatically reflected in DNS queries
  - Time To Live: TTL; not configurable for alias records;
    - the TTL assumes the value of the resource its mapped to

###### CNAME

- map one domain to another
- considerations
  - zone apex: cant create records for the zone apex
  - change detection: not supported, you have to configure a TTL
  - Time To Live:

###### SOA

###### NS

##### Routing policies

- available for all record types except NS and SOA

###### Weighted

- route traffic to mutliple resources based in specific proportions

###### geolocation

- route traffic based on request location

###### geoproxomity

- route traffic based on response location or shift traffic from one location to another

###### simple

- single resource that handles response for domain

###### latency

- route traffic to region that provides best latency

###### multivalue answer

- responds to DNS queries with up to 8 healthy records selected at random

###### failover

- configure active-passive failover

### health checks

- monitor resources ensuring responses only point to healthy resources
- you can configure notifications for faulty resources

#### monitor endpoint

- monitor an endpoint using IP addr or domain name

#### monitor other health checks

- calculated health checks are logical health checks
- OR: x or y must be healthy
- AND: x and y must be healthy

#### monitor cloudwatch alarms

- health checks based on the state of a cloudwatch alarm
- this enables all sorts of DNS failover strategies, as you can configure a cloudwatch alarm for almost anything

### resolver

- DNS resolution within a VPC without using custom DNS servers
- represented by the second IP addr in a VPC CIDR
  - any VPC resource can use to resolve records for public and private domain names
- response to DNS queries for
  - internet routable domain names
  - private hosted zone domains

## considerations

## integrations

### IAM

### ecs

- autonaming
  - manually create a namespace in a hosted zone
    - then ECS should handle the rest automagically
      - CNAME record per service autoname
      - A records per task IP
      - SRV records per task IP + port

### VPC

### cloudwatch
