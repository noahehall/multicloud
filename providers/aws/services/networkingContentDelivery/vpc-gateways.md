# VPC Gateways

- horizontally scaled, redundant, and highly available VPC components

## links

- [igw: user guide](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html)
- [ng: user guide](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [tgw: faqs](https://aws.amazon.com/transit-gateway/faqs/)
- [tgw: intro](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-network-manager.html)
- [tgw: landing page](https://aws.amazon.com/transit-gateway/?did=ap_card&trk=ap_card)
- [tgw: network manager landing page](https://aws.amazon.com/transit-gateway/network-manager/)
- [tgw: route analyzer](https://docs.aws.amazon.com/vpc/latest/tgw/route-analyzer.html)
- [tgw:network manager scenarios](https://docs.aws.amazon.com/vpc/latest/tgw/network-manager-scenarios.html)

## basics

- regional components: VPG, TG, IG, NG
- global components: DCG
- local resources: CG, LG
- route tables:
  - can be directly attached to: VPG, IG, TG
  - a local gateway default route table is created as part of the installation process of Outposts
- use cases per gateway type
  - S2S VPN:
    - customer side: always a CG,
    - AWS side: either VPG (1 active tunnel) or attached to a transit gateway (2 active tunnels)
  - Direct Connect: 3 options
    - VPG only
    - DCG + VPG
    - DCG + TG: enables east-west traffic across VPCs
  - TG: only way to connect more than 2 VPCs together without leveraging VPC peering
  - Outposts: cuz our startup is making so much fkn money you bought AWS and brought it home in a bag

## internet gateway (IGW)

- regional component attached to a VPC allowing communication between resources in a VPC and the internet.
- public vs private subnets
  - public: a route table with a route targeting an IGW providing acces to the public internet
- ingress routing: associate a route table directly with the IGW
  - segments VPC traffic
  - redirects in/outbound VPC traffic through virtual appliances
    - e.g. deploying a firewall on a EC2 > all IGW traffic is then routed through that firewall (the virtual appliance)
- Perform network address translation (NAT) for resources that have been assigned public IPv4 addresses
  - i.e. private IPs are translated into public IPs

### setup

- each VPC can have a single internet gateway that works across ALL availability zones
  - sits on the edge of your Amazon VPC, the AWS Public Zone, and the internet to manage the traffic between your Amazon VPC, the AWS Public Zone, and the internet
  - makes a subnet public by performing a type of Network Address Translation (NAT) called static NAT
- a subnet's route table must contain a route that directs Internet-bound traffic to the Internet gateway
  - can scope the route to all destinations not explicitly known to the route table (0.0.0.0/0 for IPv4)
  - can scope the route to a narrower range of IP addresses; e.g. only specific public IPv4 addresses instead 0.0.0.0/0
  - can scope the route to specific IPs of other aws resources outside your VPC

## nat gateways (NG)

- for private subnet resources to initiate contact with services outside a VPC (but not the other way around)
  - put the NAT gateway in a public subnet, and add a route targeting it in a private subnets route table
  - are subnet components, unlike IGWs which are VPC components
- performs NAT for resources assigned private IPv4 addrs
- highly available within the AZ of the subnet
- public nat gateway: can reach out to the internet, but cant be reached from the internet
  - create it in a public subnet and assign an elastic ip
  - internet access: route traffic from the nat gateway to the vpc's internet gateway
  - private access: route traffic from the nat gateway to a transit/virtual private gateway
    - this connects it to other VPCs or on premise networks
- private nat gateway: can reach out to other VPCs/on-premise networks; but not the other way around
  - route traffic from the nat gateway to a transit/virtual private gateway
  - you cannot associate an elastic ip
- public vs private NAT gateways
  - both
    - map the source private Ipv4 address of rsources to the private IPv4 address of the nat gateway
      - public: any associated internet gateway will then map the nat gateways IP address to the associated elastic ip
        - this is why public nat gateways can reach out to the internet, but private nat gateways cant
      - private: any associated internet gateway will drop outbound connections
  - public:
    - can attach an elastic ip
    - can reach out to the internet via an internet gateway
  - private
    - can be attached to an internet gateway, but it will drop outbound internet traffic

### best practices

#### gotchas

- you cannot route traffic to a NAT gateway through a:
  - VPC peering connection
  - Site to Site VPN connection
  - Direct Connect

## virtual private gateway (VPG)

- regional, logical edge routing device that sits inside the boundary of a VPC
  - the VPG is the router on the AWS side of a VPN/DirectConnect connection
- allows resources outside of your mesh network to communicate with resources inside the mesh network
  - create a VPC connection to a private network (e.g your office corporate network) enabling access to vpc resources
- ingress routing: associates a route table with a VPG
  - redirects in/outgoing traffic VPC traffic through virtual appliances
  - segments VPC traffic
- one virtual VPG per VPC:
  - VPCs cant share VPG connections: use a transit gateway instead

## Transit Gateway (TG)

- regional resource that lives outside the boundary of any specific VPC acting as a regional router between attachments
  - encrypts data automatically and never traveres over the public internet
- connect to multiple VPCs, Direct Connect, VPNs and Software-Defined Wide Area Network (SD-WAN) appliances
  - All Amazon VPCs, Site to Site VPN Connections, and Direct Connect gateways can attach to an AWS Transit Gateway hosted in their region.
  - AWS Transit Gateways in each region can be connected using peering connections that allow traffic exchange between resources and systems in different regions.
- consolidating and centrally managing routing between VPCs with a hub-and-spoke network architecture.
- provides a single global view of your private network to visualize and monitor the health of your Amazon VPCs, Transit Gateways, Direct Connect, and VPN connections to branch locations and on-premises networks.

### best practices

- peering connections can be more performance and cost effective relative to transit gateways
  - however transit gateways will be easier to manage and configure

#### anti patterns

### features

- set up a global view of your private network by registering your transit gateways and on premises resources. Your global network can then be visualized and monitored through a centralized operational dashboard.
  - monitor and manage virtual private clouds and edge connections
- connection applications across VPCs without having to manage peering connections/updating route tables
- share VPCs, DNS, active directory, IPS/IDS across regions with inter-region peering
- host multicast applications without custom hardware
- Uses the AWS Global Network for inter-Region peering and accelerated VPN to improve application performance.
  - better security with inter-region peering encryption on the aws global private network
  - Transit Gateway inter-Region peering sends inter region traffic privately over AWS global network backbone
  - Accelerated VPN uses AWS Global Accelerator to route VPN traffic from remote locations through the closest AWS edge location to improve connection performance.

#### pricing

- VPC owner
  - each hour their VPC is attached to a transit gateway
- Transit Gateway owner is billed hourly: VPN, Peering Attachments, and Transit GAteway Connect Attachments (Sd-WAN appliances)
  - total connections per hour .05
  - per GB of data .02
- AWS Direct Connect attachments: Direct Connect Gateway owner is billed hourly
- There is no additional cost for using Network Manager.

### basics

- create 1:M peering connections between VPCs, accounts, DirectConnect and on premise networks in a centralized gateway hub
- hybrid network configurations
  - a Direct Connect
  - AWS Site to Site VPN connection

#### attachments (i.e. connection types)

- remember the TG is the hub, and the attachments are the spokes
  - the big deal with TG is that it enables connections to transit the hub into the spokes
- One or more VPC attachments
- transit gateway
  - connect attachment: any compatible SD-WAN appliance
    - removes S2S vpn between HUB VPC and TG
    - dynamic routing (BGP) between SD-WAN and TG reduces routing complexity
  - peering connection with another transit gateway
- Direct Connect gateway
- S2S VPN connection to a transit gateway
- MTU by connection type
  - 8,500 bytes for: VPC, direct connect, peering
  - 1,500 bytes for VPN connections.

#### attachment association:

- the route table used to route packets coming from an attachment
- the route table can be be associated with many attachments, but an attachment can only be associated with a single route table
- default route table and custom route tables (up to 20 per TG)
  - every new attachment is automatically associated with the TG's default route table
  - on the other end, you need to go into the attachments VPC route table and add a target back to the TGs route table
- dynamic and static routes that decide the next hop based on the destination IP address of the packet.

#### route propagation:

- the route table where the attachments routes are installed
  - dynamic using BGP: VPC, S2S VPN connection, or Direct Connect gateway
  - static: VPC, peering
- on the TG side: either static or dynamic route table entries
- on the attachment side: only the VPC requires inserting static routes, all other permit dynamic routes using BGP
- path selection behavior
  - most specific route (longest prefix match)
  - static route entries, including static S2S vpn routes
  - BGP-propagated routes from Direct Connect gateway
  - BGP-propagated routes from S2S VPN

#### OSI Model

- operates at layer 3
  - packets are sent to a specific next-hop attachments, based on their destinate ip addrs

#### Network Manager

- centrally manage your networks that are built around transit gateways.
- dashboard provides a single global view of your private network.
  - centrally manage traffic across all resources, services, and accounts you have deployed within a Region
- define the resources you want to monitor
- visualize your network on a topology diagram or a geographical map
- access usage metrics and establish alerts for changes in the status of the resources you have registered
  - e.g. bytes in/out, packets in/out, packets dropped, and alerts for changes in the topology, routing, and up/down connection status, and more easily manage your entire global network.
- integrates with many software-defined networking in a wide area network (SD-WAN)
- take automated actions using Lambda functions when key events occur within a network

##### General Workflow

- Create a global network to represent your network
  - a single, private network that acts as the high-level container for your network objects.
- Register your transit gateways
  - must be in the same AWS account as your global network
  - objects auto included within registration
    - VPCs
    - Site to Site VPN connections
    - AWS Direct Connect gateways
    - Transit Gateway Connect
    - Transit gateway peering connections: you can view the peer transit gateway but not its attachments
- define and add resources
  - devices: represents the physical or virtual appliance that establishes connectivity with a transit gateway over an IPsec tunnel.
  - sites: represents the physical location of your branch, office, store, campus, or data center.
  - links: represents a single outbound internet connection used by a device, for example, a 20-Mbps broadband link.
- analyze the network with route analyzer
- monitor the network via the network manager dashboard
  - view network activity and health using CloudWatch metrics and CloudWatch Events.
  - identify whether issues in your network are caused by AWS resources, your on-premises resources, or the connections between them.

##### Route Analyzer

- designed to diagnose and resolve network disruptions quickly.
- validate new and existing routes within your AWS Transit Gateway route table
- provides a visual layout of your network traffic between AWS Transit Gateway VPCs.

##### Console

- a dashboard that helps you visualize and monitor your global network.
- overview:
  - The inventory of your global network.
  - list & status of the transit gateways that are registered in your global network
- details:
  - information about the global network resources
- geographic:
  - view the locations of the resources that are registered in your global network on a map
  - lines on the map represent connections between the resources, and the line colors represent the type of connection and their state.
- topology:
  - view, filter and explore the network tree & resource nodes for your global network.
  - displays all resources in your global network and the logical relationships between them.
- events
  - view the system events that describe changes in your global network.
  - relies on cloudwatch events
- monitoring
  - view CloudWatch metrics for the transit gateways, VPN connections, and on-premises resources in your global network.
- route analyzer
  - perform an analysis of the routes in your transit gateway route tables.

#### Hub and Spoke

- routing traffic between two or more VPCs within an AWS Region.

#### Peering

- routing traffic between VPCs or other transit gateways
- always a 1:1 nontransitive relationship

##### VPC Peering

##### Transit Gateway Peering

- a simpler network design and more consolidated management than vpc peering
- transit gateway peering connects TWO transit gateways across accounts and regions
- can open comms over the transit gatewy peering connections to all attachments
  - this will be the simplest network architecture if you have many disparate VPCs, DirectConnect and/or VPN networks

##### Inter-region peering

- functionally similar to inter-Region VPC peering
- creates a nontransitive, bidirectional route from a transit gateway in one Region to a transit gateway in another Region
- connects AWS Transit Gateways together using the AWS global network
  - automatic encryption for your data that never traverses the public internet.

## Direct Connect Gateway (DCG)

- a global resource that establishes connections that spans VPCs across multiple accounts and regions

### with VPG

- connect (associates) up to 10 VPGs globally and across accounts
  - allows north-south traffic flow
    - NOT east-east across VPCs: use a transit gateway
  - the VPG is still restricted to one region (duh, because its regional!)
- one BGP peering per DCG per direct connect connection
- 50 maximum VIFs (virtual interface) per direct connect connection

### with TG

- regional resource that lives outside of a VPC boundary
- enables east-west traffic flow across accounts, regions and VPCs
  - over one VIF and BGP peering
- manage east-west traffic via the route table of the transit gateway
  - e.g. VPC-A -> VPC-B but not VPC-B -> VPC-A

## Site to Site VPN (S2S)

### Customer Gateway (CG)

- a physical/software appliance on the customer side of a VPN connection
  - the device is represented in AWS by creating a customer gateway resource
  - the same device can be reused with multiple VPN connections
- two redundant IPSec tunnels for automatic failover
  - both tunnels must be configured on the customer gateway to work with the VPN connection
  - each tunnel contains an
    - Internet Key Exchange (IKE) security association
    - IPSec security association
    - BGP peering for dynamic routing

### VPN Gateway (VPNG)

- gateway on the AWS side of a Site to Site VPN connection

### with VPG

- VPG acts as a VPN concentrator on the AWS side of a Site to Site VPN connection
  - connect up to 10 VPN (sites) connections to a single VPG
  - consists of two tunnels, which are encrypted links supporting IPSec
    - only one tunnel can be active at anytime
    - carries a maximum of 1.25GB/sec
  - aws side: each tunnel terminates in a different AZ
    - dont forget to add a route in the VPC route table pointing to the VPG
  - customer side: both tunnels must terminate in the same customer gateway
- Border Gateway Protocol (BGP) or static routes
- redundant
  - IPSec tunnels
  - routers across two AZs

## Local Gateways (LG)

- when you require low latency access to on premise systems or to on premise data processing

### with Outposts

- only local gateway per outpost rack (you can have up to 96 racks)
- provides a target in VPC route tables for traffic destined to onpremise stuff
- performs NAT for instances that have been assigned addresses from your customer-owned IP pool

#### route tables

- aws creates a local gateway for each outpost rack and a local gateway route table as part of the installation process
- can only associate one local gateway route table with subnets that reside in the outpost

#### virtual interfaces

- aws creates one VIF for each link aggregation group (LAG)
  - then associates the VIF with the default local gateway route table
- the default route table has a defaiult route to the two VIFs for local network connectivity
