# Virtual Private Cloud (VPC)

- regionally isolated software defined virtual private network
- [direct connect](./VPC-directConnect.md)
- [gateways](./vpc-gateways.md)
- [security groups](./securitygroups.md)
- [VPC security groups](./securitygroups.md)
- [vpn](./vpc-vpn.md)

## my thoughts

- everything starts and ends with vpc

## links

- [benchmarking network throughput between ec2 instances over an IGW](https://aws.amazon.com/premiumsupport/knowledge-center/network-issue-vpc-onprem-ig/)
- [benchmarking network throughput between ec2 instances](https://repost.aws/knowledge-center/network-throughput-benchmark-linux-ec2)
- [blog: automating connectivity assessments with reachability analyzer](https://aws.amazon.com/blogs/networking-and-content-delivery/automating-connectivity-assessments-with-vpc-reachability-analyzer/)
- [blog: integrating network connectivity with CICd](https://aws.amazon.com/blogs/networking-and-content-delivery/integrating-network-connectivity-testing-with-infrastructure-deployment/)
- [cidr: prefix lists](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html)
- [cloudwatch logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-cwl.html)
- [default vpc](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)
- [DHCP: dns server for VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html)
- [ec2: elastic network interfaces](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_ElasticNetworkInterfaces.html)
- [eks: subnet tagging](https://docs.aws.amazon.com/eks/latest/userguide/network_reqs.html#vpc-subnet-tagging)
- [eks: vpc cni k8s plugin](https://github.com/aws/amazon-vpc-cni-k8s)
- [eks: vpc considerations](https://docs.aws.amazon.com/eks/latest/userguide/network_reqs.html)
- [endpoints: centralizing access blog](https://aws.amazon.com/blogs/networking-and-content-delivery/centralize-access-using-vpc-interface-endpoints/)
- [flow logs: guide](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-flow-logs.html)
- [flow logs: record examples](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-records-examples.html)
- [flow logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [intro](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html)
- [lambda: access to vpc resources](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html)
- [lambda: vpc endpoints](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc-endpoints.html)
- [landing page](https://aws.amazon.com/vpc/?did=ap_card&trk=ap_card)
- [monitoring](https://docs.aws.amazon.com/vpc/latest/userguide/monitoring.html)
- [nacl: intro](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
- [organizations: sharing vpc best practices](https://aws.amazon.com/blogs/networking-and-content-delivery/vpc-sharing-key-considerations-and-best-practices/)
- [peering: intro](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)
- [peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)
- [reachability analyzer for AWS whitepaper pdf](https://d1.awsstatic.com/whitepapers/Security/Reachability_Analysis_for_AWS-based_Networks.pdf)
- [reachability: analyzer](https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)
- [reachability: explanation codes](https://docs.aws.amazon.com/vpc/latest/reachability/explanation-codes.html)
- [route tables: examples](https://docs.aws.amazon.com/vpc/latest/userguide/route-table-options.html)
- [route tables: intro](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [route tables: walkthrough](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithRouteTables.html)
- [route tables](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Route_Tables.html)
- [s3: vpc endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html)
- [s3](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-s3.html)
- [security best practices](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-best-practices.html)
- [subnets: intro](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
- [subnets: pub & priv scenario](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html)
- [traffic mirroring](https://docs.aws.amazon.com/vpc/latest/mirroring/what-is-traffic-mirroring.html)
- [troubleshooting](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-troubleshooting.html)

### opensource

- works with traffic mirroring
  - [zeek: Network Security Monitor](https://www.zeek.org/)
  - [suricata: threat detection engine](https://suricata-ids.org/)
  - iperf3: testing bandwidth and jitter for TCP and UDP traffic
  - wireshark: detecting and decrypting network traffic

## best practices

- never use the default VPC for anything; its public by default and misconfiguration and be detrimental to various other things
  - a custom pc is private by default, and access is limited to resources within the VPC
- redundancy and fault tolerance requires at least 2 subnets across two availability zones
- saving $ on data transfer charges for NAT gateways
  - high volume GB of traffic:
    - ensure resources are in the same AZ as the nat gateway
    - or create a NAT gateway in each AZ
  - service to service comms
    - gateway endpoints for traffic destined to DynamoDB or Amazon S3.
- plan for multiple VPCs
  - you cannot create a VPN/Direct Connect/VPC peering connection between two VPCs that have matching or overlapping CIDR range
- you can now resize a VPC using a secondary CIDR range
  - you cannot associate a secondary CIDR from other ranges
  - You can only associate any other CIDR from the same primary CIDR range
- plan for load balancers in your CIDR range
  - make sure that each Availability Zone subnet for your load balancer nodes, has atleast a slash 27 bit mask
- There are tools available on the internet to help you calculate and create IPv4 subnet CIDR blocks
- VPN Connections
  - when you connect your VPCs to a common on-premises network, use nonoverlapping CIDR blocks for your networks.
  - client VPNS
    - assign a CIDR block that contains twice the number of required IP addresses to support the availability model
- peering vs transit gateways
  - transit gateways: when you have so many connections management & configuration > performance/cost
    - will incur a VPC connection and data transmission charge.
    - adds a hop between the data source and the data destination.
    - all routing tables can be configured in a centralized hub
  - VPC peering connections: when performance & costs mean more than management and configuration
    - only incur a data transmission charge
    - have no aggregate bandwidth restriction
    - does not add any additional hops.
- flow logs
  - can quickly grow into the hundreds of gigabytes.
- tag your reachability analysis to keep track of the cost

### VPC decision tree

- communicate between TWO VPCs
  - vpc peering
    - no overlapping CIDR ranges and DONT need transitive peering communication
- communicate between other VPCs or on premise using VPN
  - EC2 running VPN software
    - you manage/responsible for
      - both ends of the VPN connection
      - implementing high availability for the VPN endpoints
      - networking bottlenecks
      - pay for IGW in each VPC
  - DirectConnect
    - AWS manages/responsible for
      - managing VPN endpoints
      - multi datacenter redundancy
      - automatic failover
    - you want
      - a dedicated connection
      - simplified network management
- sharing services between VPCs across AWS Account
  - VPC PrivateLink
    - you dont want traffic to traverse the public internet

### anti patterns

## features

- secure and monitor connections, screen traffic and restrict instance access
- customize the IP address range, manage subnets and configure route tables
- enforce rules on in/outbound connections
- define network connectivity and restrictions between resources

### pricing

- check the docs
- gateways:
  - nat: each hour that a NAT gateway is available and total GB of data it processes
- peering
  - Data transferred across peering connections: per gigabyte for send and receive, regardless of the Availability Zones involved.
- endpoints
  - Interface endpoints and Gateway Load Balancer endpoints: check the privatelink docs
    - per hour the VPC endpoint remains provisioned in each Availability Zone and for each gigabyte processed through the VPC endpoint
  - gateway endpoints: free biotch!
- reachability analyzer
  - charged per analysis run between a source and a destination

## basics

- virtual network, associated to a single AWS Region
- a service that defines a boundary around the AWS services and resources and how those services and resources communicate with each other and external networks such as the internet

### Default vs Non-Default VPCs

- Default: created by AWS and are all configured in the same way
  - only one per region
  - one CIDR range that handles all i/o comms
    - VPC CIDR range is /56; VPC IPv6 CIDR for your subnet range is /64
    - VPC IPv4 CIDR 172.31.0.0/16
  - one class B subnet per AZ per region
  - A default internet gateway attached to the VPC
  - a default network ACL is created for your default Amazon VPC and is associated with all subnets.
  - default security group for instances launched within the VPC
- non default: created by you; do not allow anything in or out without explicit configuration
  - have a choice for default or dedicated tenancy
    - default: resources provisioned inside your Amazon VPC that are provisioned on shared hardware with others
    - dedicated: hardware is dedicated to you and is much more expensive.
  - have fully provisioned Domain Name Systems (DNS), which uses the Network +2 IP address
    - enableDNSHostnames: resources will be given public DNS names
    - enableDNSSupport: enable DNS resolution in your Amazon VPC.
  - VPC CIDR: between a /16 netmask (65,536 IP addresses) and /28 netmask (16 IP addresses).
    - 10.0.0.0 - 10.255.255.255 (10/8 prefix) e.g. 10.0.0.0/16 or smaller
    - 172.16.0.0 - 172.31.255.255 (172.16/12 prefix) e.g. 172.31.0.0/16 or smaller
    - 192.168.0.0 - 192.168.255.255 (192.168/16 prefix) e.g. 192.168.0.0/20 or smaller
  - subnet CIDR: same as VPC for single subnet, or smaller for multiple subnets
    - allowed blocksize is between a /28 netmask and /16 netmask.
    - CIDR blocks of the subnets cannot overlap.
    - first four & last IP addresses in each subnet CIDR block are reserved for AWS, cant be used
      - first: network address
      - second: VPC router
      - third: something to do with DHCP DNS server for VPC, check the docs
      - fourth: reserved for future use
      - last: network broadcast address (even tho its not supported in VPC)

### Architecture

- VPC: parent CIDR block
  - primary goal is to provide an isolated network for all other components
- subnets: non overlapping CIDR blocks; sub-network within the VPC cidr block
- route table: route communication between networking components
  - default route table: local route
    - enables private communication between all networking components within the associated VPC
    - never associate the defualt route table to an IGW, then ALL networking components will be publicaly accessible
  - non default route tables: each automatically copies the routes in the default route table
    - Public: destination = 0.0.0.0/0; target = IGW
      - allows inbound & outbound
    - Public via redirect: destination = 0.0.0.0/0; target = NAT Gateway/instance ID
      - allows outbound to the public internet
    - private: no routes to components with public access
- resources: located within subnets
  - public subnets: attached to a route table that is attached to an internet gateway
    - you always want NORTH-SOUTH traffic to hit the public facing resource
    - the public resource can send EAST-WEST traffic privately to resources across subnet boundaries
      - FYI: you don't need to expose resources in the public subnet at all if you use managed AWS endpoints, such as load balancers or Network Address Translation (NAT) options.
    - Bastion Host: aka jump box; a server that enables INBOUND public communication with resources in private subnets
      - server is locked down with some form of authNZ,
      - entity X can login to the server and access private resources
    - NAT Instance: enables private resources to communicate with public internet by redirecting their requests through the IGW
      - you MUST disable Source/Destination checking
  - private subnets: traffic must stay OFF the public internet
- Gateways
  - IGW: Internet gateway; enables all resources to communicate with the public internet
  - VGW: Virtual Gatway: enables direct communication from specific IPs into a VPCs private subnets
    - e.g. a data center, or VPN tunnel
- security groups: stateful firewall at the resource level: outbound automatically allows inbound responses
- NACLs: stateless stateless firewall at the subnet level: outbound DOES NOT automatically allow the inbound response
- extending a VPC to an on-premise network
  - Direct Connect
  - Site-to-Site VPN
  - Client VPN

### general workflow

- create a vpc in a specific region but spans all the AZs
  - enter CIDR range with enough IPs for available resources across subnets
- create subnets
  - attach to VPC & pick an AZ:
  - pick a CIDR range thats a subset of the VPC cidr range and doesnt overlap with other subnets or AZs
    - check the goodstuff file for notes and this [cidr visualizer](https://cidr.xyz/)
- create gateways
  - internet gateway for public subnets
    - attach it to a VPC
  - nat gateway for private subnets
  - virtual private gateway for private access
- create/adjust route table routes
  - attach to VPC
  - generally you want distinct route tables for public vs private subnets
    - only subnets with the appropriate route table connection & routes can access the internet/other vpc-local resources
    - if a subnet is public/private doesnt technically matter, its all about how the route table routes are configured
- VPC firewalls
  - create/adjust NACLs
  - create/adjust security groups

### OSI Model

- layer 3
  - routes and route tables

### subnets

- each subnet is bound to a specific AZ within a VPC and must be associated with a single route table
- the first four IPs and the last IP in that subnet are reserved by AWS.
- a subnet is made public by:
  - associating the subnet with a route table with a route to an internet gateway
    - the destination is the public internet, e.g 0.0.0.0/0
    - the target is an internet gateway
- a private subnet can reach out to the internet by:
  - associated with a NAT gateway where the destination will be 0.0.0.0/0
  - the target will be the NAT gateway
- a private subnet that cant reach out to the internet by:
  - a route table with just the default route which will be the local route
    - allows connectivity between the subnets in the same VPC
  - you can add a VPC endpoint for private connection between your VPC and other AWS services
  - you can have a route for VPC peering connection between your databases and other VPCs so that the traffic just communicates over private IPv4 address.

### route tables

- the mechanism used for controlling VPC traffic based on its target, destination, remote and local routes
  - routes internet gateway traffic to specific subnets
  - is always connected to a VPC, some type of gateway, and one/more subnets via its route destination configuration
- route: can be applied at the VPC or subnet level
  - When the CIDR blocks for route table routes overlap, the more specific (smaller range) CIDR block takes priority
- main route table: created with a VPC; allows all traffic between subnets in a vpc
  - cannot be deleted from the route table
  - implicitly used by all subnets without an explicit route table association
    - once there is an explicit association, the subnet will no longer use the main route table
- destination: where traffic thinks its going
  - CIDR range: generally this means it should match a subnet, and the target should be local
  - 0.0.0.0/0: means this is traffic to/from the internet, and the target should be some type of gateway
- target: where the traffic is actually routed to
  - local: within the VPC, and will automatically route to associated subnets
  - some gateway id
  - etc
- subnet association: this enables the routes of a routetable to be associated with resources in a subnet
  - each subnet must be associated with exaclty one route table
  - but one route table can have multiple subnets

### NACLs: network access control lists

- stateless firewall that can filter traffic as it enters and leaves a subnet.
  - you can explicitly deny traffic
  - only manage traffic that is crossing the subnet boundary.
    - i.e. has no impact on resources within the same subnet
  - do not recognize AWS resources like security groups do
    - only support ip, networks and protocols
- rules
  - traffic between nodes are two different streams, so you must edit both ingress & egress traffic
  - by default allows all in/egress traffic: you can then restrict access
  - rules are processed in order from lowest rule number to the highest rule number (ending in `*`)
    - When a rule is matched, an action is taken, and processing stops

### Peering

- enables 1:1 encrypted, highly available communication between TWO isolated VPCs using their private IP address without traversing the public internet
  - an uncomplicated and cost-effective way to share resources between Regions or replicate data for geographic redundancy.
  - each connection can accept up to 125 connections
- requirements
  - you must add a route in the respective VPC route tables
    - destination: the other VPCs cidr range
    - target: the VPC peering connection
  - the two VPCs CIDR ranges cannot overlap
- peers can span accounts and regions and There is no bandwidth bottleneck or single point of failure
- inter-Region VPC peering: peering VPCs across different AWS Regions
  - permits VPC resources that run in different AWS Regions to communicate securely with each other
  - using private IP addresses, without requiring gateways, VPN connections, or separate network appliances
- each VPC peering connection is nontransitive in nature and does not allow network traffic to pass from one peering connection to another.
  - e.g. VPC1 -> VPC2, and VPC2 -> VPC3 does not mean VPC1 -> VPC3 via VPC2
    - you have to manually connect VPC1 to VPC3

### Endpoints

- privately connect a VPC to supported AWS services and other endpoint services via PrivateLink
  - are virtual devices that scale horizontally, are highly available, and redundant
  - EAST-WEST traffic between VPC and AWS services without creating availability risks or bandwidth constraints on your network traffic.
- is a security product first
  - Traffic between the VPC and a service does not leave the Amazon network.
  - e.g. compliance requirements that prevent connectivity between a VPC and a public-facing service endpoint
- and a connectivity product second.
  - resources inside a VPC do not require public IP addresses to communicate with resources outside the VPC
  - do not require an internet gateway, virtual private gateway, network address translation (NAT) device, virtual private network (VPN) connection, or Direct Connect connection

#### Gateway Endpoints

- destinations that are reachable from within a VPC through prefix-lists within the VPC’s route table.
- used for traffic destined to DynamoDB or S3
- Instances dont require public IP addresses to communicate with VPC endpoints because interface endpoints use local IP addresses within the consumer VPC
- benefits
  - no charge for gateway endpoints
  - uses ONLY endpoint policies
  - Reduce data transfer charges resulting from outbound network communication between VPC and services that require public AWS services, such as S3
  - Security in depth using IAM, Gateway Endpoint policies, and S3 bucket policies
- setup
  - create a gateway endpoint
  - add a route table route
    - destination: service prefix
    - target: endpoint ID
  - any request to s3/dynamodb is routed through that endpoint
  - subnets associated with the route table automatically granted access to the endpoint
    - any security groups in use must add a specific rule that allows outbound traffic to the endpoint.

#### Interface Endpoints

- an elastic network interface with a private IP address from the IP address range of your subnet
- serves as an entry point for traffic destined to a supported AWS service or a VPC endpoint service.
- extend your on-premises networks to connect to your VPC and S3
  - on-premises applications can send data to the private IP of an interface endpoint.
- benefits
  - costs MO MONEY MO MONEY MO
  - Uses endpoint policies and security groups
  - Reduce data transfer charges resulting from outbound network communication between VPC and services that require public AWS services
  - Applications in an Amazon VPC can securely access AWS PrivateLink endpoints across AWS Regions using inter-Region VPC peering
  - Reduce the need to build self-managed proxy servers with private IPs for S3 access from on-premises applications
  - Can be accessed from on-premises and across Regions
- security groups
  - associated with the endpoint network interface.
  - rules control the traffic to the endpoint network interface from resources in your VPC
- using private DNS and endpoint-specific hostnames
  - keeps the traffic destined for the service contained securely within the Amazon network.
  - all traffic to the service is directed to the interface endpoint instead of through a default route

#### Gateway Load Balancer Endpoints

- an elastic network interface with a private IP address from the IP address range of your subnet.
- serves as an entry point to intercept traffic and route it to a service that you've configured using Gateway Load Balancers
- specify a Gateway Load Balancer endpoint as a target for a route in a route table

#### Endpoint Policies

- resource policies attached to endpoints: controlling access from the endpoint to the specified service.
  - manage permissions from the endpoint policy
    - you should set the s3 bucket policy to only accept connections from the endpoint
    - that way you dont need to manage TWO different policies in TWO different places
- controls the requests, users, or groups that are allowed access through the endpoint.
- default policy:
  - allows full access to the AWS service
  - If a service does not support endpoint policies, the endpoint allows full access to the service.
- custom policy
  - explicitly denies access to any actions not listed in the policy.
  - does not override or replace IAM user policies or service-specific policies (such as S3 bucket policies).
  - cannot attach more than one policy to an endpoint.

### Networking Tools

#### Traffic Mirroring

- copies inbound and outbound traffic from the network interfaces that are attached to your EC2 instances;
  - send the mirrored traffic to
    - the network interface of another EC2 instance
    - a Network Load Balancer that has a User Datagram Protocol (UDP) listener.
- use cases
  - troubleshooting
  - Content inspection
  - intrusion detection
  - treat monitoring
  - use traffic mirroring to monitor and replay production traffic in a test environment.
  - analyze for both active and passive attacks.
- similar in concept to on-premises network, packet inspection is accomplished through port mirroring on a physical switch or hub.
  - AWS only allows packets addressed for the network interface to reach it, all other traffic is filtered out, including protocols such as address resolution protocol (ARP) and broadcast traffic.
- gives you direct access to the network packets flowing through your VPC to help analyze network traffic and compare it to VPC Flow Logs to ensure the right technique for a given operations task is chosen
  - capture all traffic or you can use filters to capture the packets that are of particular interest to you, with an option to limit the number of bytes captured per packet.
  - in a multi-account AWS environment, capturing traffic from VPCs spread across many AWS accounts and then routing it to a central VPC for inspection.
  - Mirror traffic from any EC2 instance that is powered by the AWS Nitro system and 12 Xen-based instance types.
- traffic types that cannot be mirrored:
  - ARP
  - DHCP
  - Instance metadata service
  - NTP
  - Windows activation

##### Components

- traffic mirror source: ENI of an Amazon EC2 instance where AWS copies the network traffic from.
- traffic mirror target: the destination for mirrored traffic Used in more than one traffic mirror session.
  - either a network interface or a network load balancer
  - featues
    - Stream replicated traffic to any network packet collector or analytics tool, without having to install vendor-specific agents.
    - Use open-source tools or choose a monitoring solution available on AWS Marketplace.
- traffic mirror filter: rules that defines the traffic that is copied in a traffic mirror session.
  - By default, no traffic is mirrored.
  - add traffic mirror rules to the filter to define what traffic gets mirrored.
- A traffic mirror session: establishes a relationship between a source and a target that makes use of a filter.
  - A given packet is only mirrored one time
    - use multiple sessions on a source for multiple mirrors
    - useful if you want to send a subset of the mirrored traffic from a traffic mirror source to different tools.
  - evaluated based on the ascending session number that you define when you create the session. The first match (accept or reject) is used to determine the fate of the packet.
- connectivity considerations
  - If the traffic mirror source and the traffic mirror target is owned by:
    - same account
      - traffic can be in
        - the same VPC
        - a different VPC connected through an intra-Region VPC peering or a transit gateway.
    - different accounts
      - The traffic mirror target owner must share the target with the traffic mirror source account using the Amazon Resource Access Manager (Amazon RAM).
      - the mirrored traffic is sent to the traffic mirror target using the source Amazon VPC route table
- setup process
  - identify the traffic mirror source
  - configure the traffic mirror target
  - configure the traffic mirror filter for the source ec2 instance
  - configure the traffic mirror sessions for the source, filter, and target
  -

#### Reachability Analyzer

- easy way to examine network connectivity between two endpoints within your VPC without sending any packets
- analyzes all possible paths through your network without having to send any traffic on the wire. It looks at the configuration of all resources in your Amazon VPCs to determine what network flows are feasible.
- troubleshoots reachability between two endpoints in an Amazon VPC, or within multiple Amazon VPCs.
  - All resource configurations (security groups, routes, firewalls, etc) that can affect the connectivity of your network are inspected to determine if the network flow is possible.
  - VPN gateways
  - Network interfaces
  - Internet gateways
  - VPC endpoints
  - VPC peering connections
  - Transit gateways
- use cases
  - should be integrated into your network design process
  - configuration analysis tool that enables you to perform connectivity testing between a source resource and a destination resource in your virtual private clouds
  - Network diagnostics tool that troubleshoots reachability between two endpoints.

##### General Workflow

- create a path: from traffic source to destination
  - endpoint types: VPN Gateways, Instances, Network Interfaces, Internet Gateways, VPC Endpoints, VPC Peering Connections, and Transit Gateways.
  - connectivity options:
    - TCP/UDP + port number
    - IP addrs
  - sources & destinations
    - owned by the same account
    - in the same region
    - either
      - within the same VPC
      - different VPCs but connected via VPC peering
      - Shared VPC owned by the same account
- analyze the path: repeat whenever the configuration changes
  - displays the details and identifies the blocking component
  - identifies the blocking component.
- review results
  - destination
    - reachable: produces hop-by-hop details of the virtual network path between the source and the destination.
      - multiple reachable paths: identifies and displays the shortest path
    - not reachable: identifies the blocking component.
      - possibly caused by configuration issues in a security group, network ACL, route table, or load balancer.
      - explanation codes:

#### flow logs

- capture info about ip traffic and publish to cloudwatch logs, s3 or kinesis
  - logs do not capture IP traffic to and from Amazon reserved IPs
- track and trigger cloudwatch alarms based on changes in:
  - flow duration
  - latency
  - traffic type
- centralized source to monitor different network aspects and to provide a history of network traffic flows within entire Amazon VPCs, subnets, or specific elastic network interfaces (ENIs).
- data is collected outside of the path of your network traffic, and therefore does not affect network throughput or latency. You can create or delete flow logs without any risk of impact to network performance.
- tool for identifying problems with your network's traffic
  - Tracing network activity to a specific IP address
  - troubleshooting why specific traffic is not reaching an instance,
  - a security tool to monitor the traffic, profile your network traffic, and to look for abnormal traffic behaviors.
- primary use cases
  - performance: provides flow duration, latency, and bytes sent and can be used to identify latencies, establish performance baselines, and improve applications.
  - security: log all traffic from an Amazon VPC, an interface, or a subnet for root cause analysis to identify gaps in your security.
  - compliance: how that your organization is compliant with specific industry, federal, state, and local regulations that your organization must follow; publish to s3 for historical audits
- configuration: once a flow log is created, it can be updated: it must be deleted and created anew
  - traffic filter type: all, accepted, rejected traffic
  - log name: specify a functional name for the log
  - destination: where to publish flow log data; s3 or cloudwatch
  - permissions: the log owner requires IAM privleges to publish and work with flow log data
- Flows are collected, processed, and stored in capture windows that are approximately 10 minutes long
  - create up to 2 flow logs per resource
- publishing to s3 vs cloudwatch
  - cloudwatch: data is published to a log group; Log streams contain flow log records
    - Search and analyze the log data with CloudWatch Logs Insights to perform queries and to respond efficiently and effectively to operational issues.
    - Subscribe to a Kinesis Stream for analysis with AWS Lambda.
  - s3: using a folder structure that is determined by the flow log's ID, Region, and the date on which they are created.
    - Scalability and log consolidation using Amazon Athena to query the data for analysis.
    - ingestion from pipelines using S3 bucket notifications, Amazon SQS, and AWS Lambda to provide events to Amazon Elasticsearch Service for real-time monitoring using Kibana.
- monitoring complex multi-ip addrs or mult-interface ec2s
  - EC2 with multiple IP addresses:
    - recorded under the primary private IP address, listed in the dstaddr field for the EC2 network interface
    - configure the flow log to use the pkt-dstddr log field To separate log traffic by destination IP
  - intermediate devices e.g. a NAT gateway
    - traffic sent
      - record the IP of the intermediate device in the srcaddr field
      - configure the flow log to use the pkt-srcaddr field To ensure that the original source IP address of device that generated the packet is recorded
    - traffic received
      - record the IP of the intermediate device in the dstaddr field
      - configure the flow log to use the pkt-dstaddr field To ensure that the original destination IP address is recorded

##### Records

- log events consisting of fields that describe the traffic flow
  - vpc level: activity of your operations within your cloud environment.
  - subnet level: activity for a specific subnet.
  - network interface level: specific interfaces on ec2 instances and capture flow logs from that interface.
- inspect flow logs:
  - remeber that the flow of a flow log is a subset of a session and describes the number of packets moving in one direction
    - thus you need to inspect a series of flow logs to understand the request-respone between parties
  - you can identify sessions if the SOURCE + DEST IPs and PORTS dont change across flow records

```sh

# if a field is not applicable/computed for a record, its marked by `-`
ACCOUNT-ID ENI-ID SOURCE-IP DEST-IP SOURCE-PORT DEST-PORT PROTOCOL PACKETS - - - ACTION -
# account-id: owner of the network interface
# ENI-ID: id of the network interface
# SOURCE-IP: of incoming traffic / ipv4/6 of the ENI of the outgoing interface
# DEST-IP: of outgoing traffic / ipv4/6 of ENI for incoming traffic on the interface
# SOURCE-PORT: used by the transmitting system
# DEST-PORT: expected to receive this traffic
# PROTOCOL: [this link for number assignments](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml)
# PACKETS: number of packets transferred during the flow
# ACTION: e.g. accept/reject, taken by the security groups, NACLs, etc
```

### encryption

- hardware acceleration allows for line-rate AES-256 encryption of network traffic without performance penalty
- automatically for all VPC, and VPC-peering traffic

## considerations

- vpc ip range: 1 primary and up to 4 secondary; the smallest range is 28 (4 ips), the largest is 16 (65,536 ips)
  - 10.1.0.0/16
  - 192.168.0.0/16
- region: should be wherever you plan to launch resources
  - the select ip range are distributed at the region level
- subnets
  - each in a availability zone where you expect to launch resources
  - CIDR range thats a subset of the VPC cidr range
    - you generally increment the third octet choose an appropriate flexbit
      - use the visualizer link in the goodstuff doc
- peering
  - cannot create a VPC peering connection between VPCs with matching or overlapping IPv4 CIDR blocks
    - even if you intend to use the connection for IPv6 communication only
  - you cannot extend a peering relationship to a connection If either VPC has one of the following connections
    - A VPN connection or a Direct Connect connection to a corporate network
    - An internet connection
      - through an internet gateway
      - in a private subnet through a NAT device
    - A gateway VPC endpoint to an AWS service, for example, an endpoint to Amazon S3

## integrations

- many integrations can be more secured by communicating with other services via VPC Endpoint
- use a Network Load Balancer and an interface endpoint to share an application from a VPC.

### EC2

- on ec2 creation, you can select an existing VPC and subnet to launch an instance into under network settings
- Each instance is configured with a primary network interface; which is a logical virtual network card.
  - receives a primary private IP address from the IPv4 address of the subnet
    - that private IP address is assigned to the primary network interface.
  - can add a public IP address.
    - ephemeral: associated with your instance until it is stopped, rebooted, or terminated
    - static: allocate an elastic IP address for your AWS account and associate that elastic IP address with an instance or a network interface

### rds

- db instances require a VPC with two private subnets not connected to an internet gateway
  - you can further protect your db by using NACLs and security groups

### eks

- you either create a new VPC or use an existing one
- generally follow one of three patterns
  - only public subnets: at most 3 public subnets in distinct AZs
    - all worker nodes automatically receive public IPs and can send/receive internet traffic through an internet gateway
    - a security group is deployed that denies all inbound traffic, but allows outbound traffic
  - only private subnets: at most 3 private subnets in distinct AZs
    - all nodes can optionally send/receive internet taffic through a NAT instance or NAT gateway
      - a NAT instance/gateway must be deployed to each AZ
    - a security group is deployed that denies all inbound traffic, but allows outbound traffic
  - public and private subnets: 2 AZs, each with a private and public subnet
    - recommended for all production deployments
    - private subnet: deploy worker nodes
      - dont receive public IPs
        - worker nodes can still communicate with the cluster and other aws services
        - pods can communicate outbound to the internet througha NAT gateway deployed in each AZ
      - a security group is deployed that denies all inbound traffic, but allows outbound traffic
    - public subnet: deploy load balancers for balancing traffic in private subnets
      - all resources automatically receives public IP addrs
      - you must ensure all subnets are tags so k8s knows which are public and private

#### vpc cni plugin

- allows k8s pods to have the same IP addr inside the pod as they do on the VPC network
  - enables interhost communication and allows k8s pods to have the same ip addr inside the pod as they do on the VPC network
- provisions multiple network interfaces to a host instance
  - each network interface has multple secondary ip addrs that get asigned IP addrs from the VPC pool
  - the ip addrs are assigned to pods on the host and connects the network interfac eto the veth port created on the pod
- every pod has a real, routable ip addr from the VPC and can easily communication with other pods, nodes or aws services
- on the host ec2 instance
  - cni modifies both the default routing table and the network interface routing table
    - default routing table: routes traffic to pods
    - network interface: has its own routing table for routing outgoing pod traffic
  - each pod is assigned one of the network interfaces secondary ip addr

### Elastic Load Balancers

- load balancing across resources in multiple subnets
- great to pair with an AWS Auto Scaling Group to enhance the high availability, fault-tolerance, and scalability of an application.

### Systems Manager

- configuring Systems Manager to use an interface VPC endpoint
- AWS PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network.

### Cloudwatch

- using traffic mirroring, vpc flow logs + cloudwatch
- configure VPC Reachability Analyzer with CloudWatch to alert on connectivity issues and possibly automatically remediate using AWS Lambda.
