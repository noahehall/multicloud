# PrivateLInk

- private connectivity between Amazon VPCs, AWS services, and onpremise networks without traversing the public net

## my thoughts

## links

- [landing page](https://aws.amazon.com/privatelink/?did=ap_card&trk=ap_card)
- [accessing services through privatelink](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html#what-is-privatelink)
- [cloudformation: setting up vpc endpoints](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-vpce-bucketnames.html)

## best practices

- cheaper for aws to aws (east west) traffic compared to going over the public internet
- best practice to always use privatelink when possible for improved VPC security posture

### anti patterns

- alternatives
  - Private IP addresses using VPC peering
    - but that may be more management overhead than necessary
    - VPC Peering connections are only a one-to-one connection.
  - connect VPCs via Public IP addresses using the internet gateway of the VPC

## features

- secure traffic via private IP addresses
- simplified network & firewall management rules
- HIPAA, EU-US privacy shield, PCI and other governmental compliancy regulations
- deliver SaaS services via prebuilt third-party integrations
- connect services across different accounts and Amazon VPCs to significantly simplify your network architecture
- use private IP connectivity: services function as though they were hosted directly on your private network.
- associate security groups and attach an endpoint policy to interface endpoints, control who access to a specified service

## basics

- services establish a TCP connection between VPCs for private access to services across VPC boundaries
  - other accounts and VPCs can create VPC endpoints to access your endpoint service
  - endpoint services can be either network or gateway load balancers
- Traffic will be sourced from the Network Load Balancer inside the service provider VPC
  - provider perspective:
    - all IP traffic will originate and appear as private ips addrs from the Network Load balancer
    - the providers application will never see the IP addresses of the customer or service consumer.

### Proxy Protocol v2

- gain insight into the network traffic.
- Network Load Balancers use Proxy Protocol v2 to send additional connection information such as the source and destination

### DNS

- endpoint-specific DNS hostnames: used to communicate with the service

  - e.g. `vpce-0fe5b17a0707d6abc-29p5708s.ec2.us-east-1.vpce.amazonaws.com`

- zonal-specific DNS hostnames: specific to an AZ and support cross-zone load balancing to distribute traffic across registered targets in all activated Availability Zones.
  - manually generated and incur egional data transfer charges for any data that is transferred between AZs
  - e.g. `vpce-0fe5b17a0707d6abc-29p5708s-us-east-1a.ec2.us-east-1.vpce.amazonaws.com`
- Private DNS Hostnames: alias zonal-specific or regional-specific DNS hostnames into a friendly hostname e.g. `myservice.ogobar.com`

### OSI Model

- layer 4: network load balancer
- layer 3: gateway load balancer

### Endpoint types

- both uses private IP addresses and security groups within a VPC so that regionally hosted services function as though they were hosted directly within a VPC.

#### Interface

- connect you to services hosted by AWS Partners and supported solutions available in AWS Marketplace.

#### Gateway Load Balancer

- Gateway Load Balancer endpoints: security and performance for virtual network appliances or custom traffic inspection logic.

## considerations

- does not support IPv6
- Endpoint services cannot be tagged.
- The private DNS of the endpoint does not resolve outside of the VPC
  - DNS hostnames can be configured to point directly to endpoint network interface IP addresses
  - Endpoint services are available in the Region in which they are created and accessed using VPC peering.
- Availability Zone names may not map to the same geographic location across accounts
  - i.e. account-A US-East-1A !== account-B US-East-1A even tho they use the same name

## integartions

### cloudformation

- configure cloudformatio to use an interface VPC endpoint

### api gateway

- use with the REST API private endpoint
