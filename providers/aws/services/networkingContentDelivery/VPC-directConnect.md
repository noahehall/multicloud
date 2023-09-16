# Direct Connect

- Create a private and dedicated network connection from on-premise to AWS

## my thoughts

## links

- [landing page](https://aws.amazon.com/directconnect/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

- Improve application performance by connecting directly to AWS and bypassing the public internet.
- Secure your data as it moves between your network and AWS with multiple encryption options.
- Build hybrid networks: Link your AWS and on-premises networks to build applications that span environments without compromising performance.
- Extend your existing network: use SiteLink to send data between your locations. When using SiteLink, data travels over the shortest path between locations.
- Manage large datasets: data transfers at massive scale for real-time analysis, rapid data backup, or broadcast media processing.

### pricing

- connecting to resources running in any AWS Region or Local Zone, there are three factors that determine pricing:
  - capacity: maximum rate that data can be transferred through a network connection
    - measured in megabit per second (Mbps) or gigabit per second (Gbps): 1 Gbps === 1,000 megabits per second (1,000 Mbps).
  - port hours: measure the time that a port is provisioned for your use
    - Even when no data is passing through the port, you are charged for port hours
      - dedicated: physical connections between your network port and an AWS network port inside an AWS Direct Connect location.
        - billed as long as that port is provisioned for your use
        - request a dedicated connection through the AWS Direct Connect section of the AWS Management Console.
      - hosted: logical connections that an AWS Direct Connect Delivery Partner provisions on your behalf
        - you connect to the AWS network using one of the partner’s ports
        - request a hosted connection by contacting an AWS Direct Connect Delivery Partner directly.
  - data transfer out (DTO): the cumulative network traffic (the amount of data transferred, not the speed) that is sent through AWS Direct Connect to destinations outside of AWS
    - charged per gigabyte (GB): depends on the AWS Region or AWS Local Zone, and the AWS Direct Connect location
- Connecting between locations using SiteLink: theres an additional cost
  - SiteLink hours
  - SiteLink data transfer

## basics

- Letter of Authorization and Connecting Facility Assignment: LOA-CFA
  - proves AWS has authorized the completion of the last physical step for your Direct Connect connection.
  - share your LOA-CFA with your Direct Connect Partner and they will make this connection
- Link Aggregation Control Protocol: LACP
  - facilitating multiple dedicated physical connections to be grouped into LAGs
  - you can stream the multiple connections as a single, managed connection.
- Link Aggregation groups: LAGs
  - can have a maximum of two 100-Gbps connections in a LAG, or four connections with a port speed less than 100 Gbps
  - All connections in the LAG
    - must use the same bandwidth.
    - must terminate at the same Direct Connect endpoint.

### Connection Types

- dedicated connection collocated at a Direct Connect location
  - AWS partnered with companies to offer physical uplinks to AWS
  - you find a Direct Connect location and deploy a router and supporting equipment
  - you are responsible for
    - The deployed equipment
    - the circuit that will connect your on-premises location to the deployed equipment,
    - the connection from the deployed equipment to the AWS router.
- Direct Connect Partner
  - they already have equipment at a direct connect location
  - you will need to provide the physical connection between your on-premises location and the Direct Connect Partner equipment
  - the Direct Connect Partner will configure and maintain the physical equipment at the Direct Connect location.
- Direct Connect node
  - a direct physical connection from your on-premises location to a Direct Connect node.
  - bet this shiz costs a fk ton

### Virtual Interfaces

- the type of physical hardware that makes the connection determines the type of virtual interface is supported

#### Private

- lets you connect to all virtual private cloud, or VPC, resources within the private IP space in your AWS environment
- Connect a single private virtual interface to multiple VPCs through private gateways within an AWS Region by associating it with your Direct Connect gateway.
- permits traffic to be routed to any VPC resource in the same private IP space as the virtual interface.

#### Public

- lets you route traffic to all VPC resources with a public IP address or that are connected to an AWS public endpoint.
- you can connect to all public global AWS IP addresses and access AWS global IP route tables.
- permits traffic to be routed to any VPC or AWS regional resource with a public IP address in the same Region.

#### Transit

- lets you connect your Direct Connect connection to AWS Transit Gateway
- permits traffic to be routed to any VPC or AWS regional resource routable through an AWS Transit Gateway in the same Region.
- use the AWS Transit Gateway Network Manager to manage the traffic moving between your AWS environment and your physical location
- supports connecting three transit gateways to your Direct Connect gateway.

### OSI Model

- operates through layer 1 to layer 4
- provides virtual access to physical, data link, network and transport

## considerations

- All equipment part of the physical connection linking your location with AWS must support 802.1Q encapsulation.
- Ethernet connections must all be single-mode fiber
  - one gigabit per second
    - require a 1,310-nanometer 1000BASE-LX transceiver.
  - 10 gigabits per second
    - require a 1,310-nanometer 10-gigabit BASE-LR transceiver.
  - 100 gigabits per second: for applications that transfer large-scale datasets
    - require a 100-gigabit BASE-LR4 transceiver.
    - only available in specific regions
- must disable auto-negotiation and configure your port speed and full duplex mode
- The router that will connect to the AWS router must support
  - Border Gateway Protocol (BGP)
  - Border Gateway Protocol MD5 authentication.
- BGP requires a public or private Autonomous System Number (ASN)
  - Private ASNs can be self-determined
  - Public ASNs must be purchased and registered.
- Virtual Interfaces
  - support a default Ethernet frame size of 1522 bytes and a jumbo Ethernet frame size of 9023 bytes
  - private VI: reqiure a private ASN
  - public VI: require a public ASN
  - transit VI:

## integrations
