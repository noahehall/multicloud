# VPC Gateways

- horizontally scaled, redundant, and highly available VPC components

## links

- [igw: user guide](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html)
- [natg: user guide](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [tgw: faqs](https://aws.amazon.com/transit-gateway/faqs/)
- [tgw: intro](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-network-manager.html)
- [tgw: landing page](https://aws.amazon.com/transit-gateway/?did=ap_card&trk=ap_card)
- [tgw: network manager landing page](https://aws.amazon.com/transit-gateway/network-manager/)
- [tgw: route analyzer](https://docs.aws.amazon.com/vpc/latest/tgw/route-analyzer.html)
- [tgw:network manager scenarios](https://docs.aws.amazon.com/vpc/latest/tgw/network-manager-scenarios.html)

## internet gateway (IGW)

- regional component attached to a VPC allowing communication between resources in a VPC and the internet.

### features

- Provide a target in route tables to connect to the internet
  - this makes the subnet a public subnet
- ingress routing: associate a route table directly with the IGW
  - segments VPC traffic
  - redirects in/outbound VPC traffic through virtual appliances
    - e.g. deploying a firewall at the IGW would be a virtual appliance
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

## Customer Gateway (CG)

- physical/software appliance you own/manage in your onpremise network

## VPN Gateway (VPNG)

- gateway on the AWS side of a site-to-site VPN connection

## Direct Connect Gateway (DCG)

- establishes connections that spans VPCs across multiple regions

## nat gateways (NG)

- for private subnet resources to initiate contact with services outside a VPC (but not the other way around)
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

## virtual private gateway (VPG)

- logical edge routing device that sits at the edge of a VPC
- allows resources outside of your mesh network to communicate with resources inside the mesh network
- can be used with DirectConnect and VPNs
  - act as a VPN concentrator on the AWS side of a site-to-site VPN connection
  - create a VPC connection to a private network (e.g your office corporate network) enabling access to vpc resources

## Transit Gateway

- connect to multiple VPCs, Direct Connect, VPNs and Software-Defined Wide Area Network (SD-WAN) appliances
  - All Amazon VPCs, Site-to-Site VPN Connections, and Direct Connect gateways can attach to an AWS Transit Gateway hosted in their region.
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
    - Site-to-Site VPN connections
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

##### Inter-region peering

- functionally similar to inter-Region VPC peering
- creates a nontransitive, bidirectional route from a transit gateway in one Region to a transit gateway in another Region
- connects AWS Transit Gateways together using the AWS global network
  - automatic encryption for your data that never traverses the public internet.

## integrations
