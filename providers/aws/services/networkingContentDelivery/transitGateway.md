# Transit Gateway

- connect to multiple VPCs, Direct Connect, VPNs and Software-Defined Wide Area Network (SD-WAN) appliances
- consolidating and centrally managing routing between VPCs with a hub-and-spoke network architecture.

## my thoughts

## links

- [landing page](https://aws.amazon.com/transit-gateway/?did=ap_card&trk=ap_card)
- [network manager](https://aws.amazon.com/transit-gateway/network-manager/)
- [faqs](https://aws.amazon.com/transit-gateway/faqs/)

## best practices

- peering connections can be more performance and cost effective relative to transit gateways
  - however transit gateways will be easier to manage and configure

### anti patterns

## features

- monitor and manage virtual private clouds and edge connections
- better security with inter-region peering encryption on the aws global private network
- connection applications across VPCs without having to manage peering connections/updating route tables
- share VPCs, DNS, active directory, IPS/IDS across regions with inter-region peering
- host multicast applications without custom hardware

### pricing

- VPC owner
  - each hour their VPC is attached to a transit gateway
- Transit Gateway owner is billed hourly: VPN, Peering Attachments, and Transit GAteway Connect Attachments (Sd-WAN appliances)
  - total connections per hour .05
  - per GB of data .02
- AWS Direct Connect attachments: Direct Connect Gateway owner is billed hourly

## basics

- create 1:M peering connections between VPCs, accounts, DirectConnect and on premise networks in a centralized gateway hub
- hybrid network configurations
  - a Direct Connect
  - AWS Site-to-Site VPN connection
- attachments (i.e. connection types)
  - One or more VPCs
  - compatible Software-Defined Wide Area Network (SD-WAN) appliance
  - Direct Connect gateway
  - peering connection with another transit gateway
  - VPN connection to a transit gateway
- MTU by connection type
  - 8,500 bytes for: VPC, direct connect, peering
  - 1,500 bytes for VPN connections.
- route table: can be be associated with many attachments, but an attachment can only be associated with a single route table
  - default route table and can optionally have additional route tables.
  - dynamic and static routes that decide the next hop based on the destination IP address of the packet.
  - target of these routes can be any transit gateway attachment.
- route propagation
  - dynamic: A VPC, VPN connection, or Direct Connect gateway
  - static: VPC, peering

### OSI Model

- operates at layer 3
  - packets are sent to a specific next-hop attachments, based on their destinate ip addrs

### Hub and Spoke

- routing traffic between two or more VPCs within an AWS Region.

### Peering

- routing traffic between VPCs or other transit gateways
- always a 1:1 nontransitive relationship

#### Network Manager

- dashboard provides a single global view of your private network.
  - centrally manage traffic across all resources, services, and accounts you have deployed within a Region
- define the resources you want to monitor
- visualize your network on a topology diagram or a geographical map
- access usage metrics and establish alerts for changes in the status of the resources you have registered
- integrates with many software-defined networking in a wide area network (SD-WAN)

#### VPC Peering

#### Transit Gateway Peering

- a simpler network design and more consolidated management than vpc peering

#### Inter-region peering

- functionally similar to inter-Region VPC peering
- creates a nontransitive, bidirectional route from a transit gateway in one Region to a transit gateway in another Region
- connects AWS Transit Gateways together using the AWS global network
  - automatic encryption for your data that never traverses the public internet.

## considerations

## integrations
