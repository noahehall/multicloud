# networking

- CSMA: carrier-sense multiple access
- Software Defined Networks (SDN)
  - an approach to networking that uses software based controllers/APIs to communicate with underlying infrastructure and direct traffic
  - a group of software services working together to create a network construct
- Network design documents
  - documents and diagrams visualize the components of a network, including routers, firewalls, and devices
  - show how those components interaca

## Protocols

- IP: address denoting a specific computer on the internet
  - first part: network; used to identify the network part within the network
  - last part: host; used to specify a specific host within that network.
- IPv4: a 32bit ip address converted to decimal format: 4 octets, each representing 8 bits (0-255)
  - public:
    - Class A starts at 0.0.0.0 and ends at 127.255.255.255
    - Class B starts at 128.0.0.0 and ends at 191.255.255.255
    - Class C, starts at 192.0.0.0 and ends at 223.255.255.255
    - there are also Class D and Class E IP ranges
  - private: The default Amazon VPC in AWS is configured using a Class B range.
    - Class A is 10.0.0.0 - 10.255.255.255 providing one single Class A IP addresses.
    - Class B is 172.16.0.0 - 172.31.255.255 providing 16 Class B IP addresses.
    - Class C is 192.168.0.0 - 192.168.255.255 providing 256 Class C IP addresses.
    - for private IPs to connect with the public internet, they have to use Network Address Translation
- IPv6: 128bit ip address represented in hexadecimal rather than the dotted decimal like IPv4 addresses

### Network Communication

- determine the formats and rules used to transfer data over the network
- handle authentication and error detection, syntax, synchronization, and semantics for both hardware and software
- examples
  - HTTP
  - TCP: defines how to establish and maintain a network conversation by which applications can exchange data
  - UDP: primarily used to establish low-latency and loss-tolerating connections between applications on the internet
  - IRC

### Network Management

- define the policies and procedures used to monitor, manage, and maintain your network
  - Troubleshoot connections between host and client devices.
  - connection's status, availability, packet or data loss, and so on related to the health of your network connection.
  - ensures stable communication and optimal performance of the network
- can be applied to all devices on your network (computers, switches, routers, and servers).
- examples
  - SNMP: monitor and manage network devices
  - ICMP: used for diagnostic purposes, send error messages and inspect connectivity issues between devices

### Network Security

- define how the network secures data from malicious attacks
  - protects the data from unauthorized users, services, or devices that access your network data
  - rely on encryption and cryptography to secure data.
- examples
  - SSL
  - SFTP
  - HTTPS

### Network design and configuration

- a combination of network management, communication, and security protocols.

## CIDR notation

- classless inter-domain routing; enables expressing a rangte of ip addresses
- provides a numerical representation of a network that describes its characteristics and mask length which determines the usable addresses, including the start and end addresses
- addresses are represented by the starting IP address of the network, called the network address, and the prefix, which is a forward slash and a number that represents the size of the network.

### Subnetting

- the process of dividing a network into smaller logical networks that exist within a single Class A, B, or C network to isolate groups of hosts together
  - each data link on a network must have a unique sub/network ID.
    - with every node on that data link being a member of the same network (or subnet)
    - Any device, or gateway, that connects N sub/networks has N distinct IP addresses, one for each sub/network that it interconnects.
  - improve routing efficiency, network management control, and network security
    - network traffic can travel a shorter distance without passing through unnecessary routers to reach its destination
- subnet: i.e. sub-network; a logical organization of connected network devices
- subnet mask: important for IPv4 addresses because the IP address doesn't give any information on the network size
  - for internal usage within a network to determine if a host is on the local or remote subnet
    - e.g. 205.0.125.100
    - class A: network 205, host 0.125.100
    - class B: network 205.0, host 125.100
    - class C: network 205.0.125, host 100

```sh
# a single ip
192.168.1.0

# a range of ips in which the first 24 are fixed, and the last 8 are flexible
# 32 - 24 = 8; the remaining 8 bits can be assigned to the 0 in 0/24
# the flexible bits can be either a 0 or 1, 2 choices for each of the 8 bits
## 32 - 24 = 8; 2^8 = 256 total ip addresses
## netmask: 255.255.255.0
## cidr base ip: 192.168.0.0
## broadcast ip: 192.168.0.255
## first useable: 192.168.0.1
## last usable: 192.168.0.254
192.168.0.0/24
# same as above, except
## 32 - 16 = 16; 2^16 = 65526 total ip addresses
## netmask: 255.255.0.0
## broadcast ip: 192.168.255.255
## last usable: 192.168.255.254
192.168.0.0/16

# 1024 ip addresses
192.168.0.0/22

```

## routing

- Routing is how your data moves:
  - across your network from device to device and provides the path to deliver network packets from the source to the destination
  - across the internet from your local network
- Global networking: moves your data across the internet through many interconnected networks
- Local networking: takes that data that is broken up into multiple pieces for reliable transport.

## OSI Model

- Open Systems Interconnect Model: a logical model and it was designed to describe the functions of the communication system by dividing the communication procedure into smaller and simpler components

### Media Layers

- define how our data moves between point A and point B
  - Point A could be in your local network, and point B too, or maybe point B is across the internet.

#### Physical (layer 1)

- helps the devices on the network communicate and provides transmission and reception of raw bit streams over a physical medium
- eventually physical cables are used to transmit unstructured data

#### Data Link (layer 2)

- provides reliable transmission of data frames between two nodes connected by a physical layer
- adds more function and intelligence and to provide device-to-device communication
  - frames: format for sending information and data over a layer 2 network
  - mac addresses: unique hardware addresses for identifying devices on the network
  - controlled access to the layer 1 physical medium
  - collision detection: CD; reduces collisions along with improvements CSMA

#### Network (layer 3)

- responsible for moving the data from the source to the destination
- has no reliable method for ensuring packet delivery
- adds
  - IP: assigns cross network address to devices on the network
    - defines how computers send packets of data to each other
    - once addresses are assigned, it uses routing to communicate across networks
    - packets: containers that encapsulate the raw data being sent
    - routes: move packets of data through networks by reviewing and checking route tables
    - route tables: help the transmission for the routesrs to forward packets

### Host Layers

- where your data is broken up for transport and then reassembled when it reaches the destination

#### Transport (layer 4)

- adds the functionality to support the networking used on the internet
- structuring and managing of the network: addressing, routing, and traffic control
  - ports:
  - segments:
  - error correction: sequence number to ensure the order of segments are maintained
  - retransmmission: if a packet is lost in transit, it can be resubmitted
  - flow control and a connection oriented architecture:
    - ensures you can create a connection between a client and sever using a three-way handshake
- adds protocols
  - TCP
  - UDP: primarily used to establish low-latency and loss-tolerating connections between applications on the internet
- stateless firewalls: do not understand the state of a connection
  - outbound traffic: leaving the client
  - inbound/response traffic: leaving the server

#### Session (layer 5)

- manages communiation sessions between two nodes
- stateful firewalls: understand the state of a connection & tcp segments
  - outbound traffic: leaving the client, if accepted, automatically accepts the response from the server
  - inbound/response traffic
  - can create bidirectional communication between client & sever or any two devices

#### Presentation (layer 6)

- adds features for the delivery and formatting of the information and further processing/display to layer 7
  - separation of different data representation
  - encryption
  - transforms the data to & from an application format and a network format
- sometimes presentation features can be performed in the application layer
  - hence the presentation layer can sometimes be skipped

#### Application (layer 7)

- supports communications for end-user processes and applications
- handles the presentation of data for user-facing software applications
- generally all application-specific functionality
  - identifying communication partners and the quality of service between them
  - determining resource availability, privacy and user authentication
  - synchronizing communication
  - connects this layer to lower layers in the OSI model
- common protocols: http, smtp, ftp, web browsing, top level API calls (e.g. REST)

## TCP/IP

- Transmission Control Protocol/Internet Protocol: designed for standard protocols and is a subsection of the OSI model
  - aka the internet protocol suite.
  - set of rules and procedures that function as an abstraction layer between internet applications and the routing and switching.
    - specifies how data is exchanged over the internet: how data should be broken into packets, addressed, transmitted, routed, and received at the destination
- It combines the seven layers of the OSI model into four layers

### Link (Layer 1)

- Defines the networking methods within the scope of the local network link on which hosts communicate without intervening routers.
- a combination of the data link and physical layer of the OSI model
- features
  - Hardware Addressing: MAC Addresses
  - Protocols present to allow the physical transmission of data
- Protocols: that only operate on a link
  - Ethernet
  - ARP: Address Resolution Protocol

### Internet (Layer 2)

- responsibility for sending packets across network boundaries.
- covers the functions of the network layer in the OSI model
- Establishes basic data channels that applications use for task-specific data exchange.
- defines protocols responsible for the logical transmission of data over the entire network
- connectins independent network and transports packets across network boundaries
- protocols
  - IPv4/6
  - ICMP
  - ARP

### Transport (Layer 3)

- Establishes basic data channels that applications use for task-specific data exchange.
- covers the functions of the transport layer of the OSI model
- features
  - handles to end-to-end communication
  - provides flow control
  - ensures reliability with error-free delivery of data
- protocols
  - TCP
  - UDP

### Application (Layer 4)

- Provides end user service, like exchanging application data over the network connections established by the lower level protocols.
- covers layer 5, 6 and 7 of the OSI model
- features
  - provides standardized data exchange
  - handles node-to-node communications
  - controls user-interface specifications
- protocols
  - HTTP
  - ssh: cryptographic network protocol for operating network services securely over an unsecured network.
  - NTP
  - ftp
  - POP: post office protocol
  - SMTP
  - SNMP

## Networking Components

- gateways vs routers: are usually separate devices, but modern routers can function as gateways
  - gateways connect networkers
  - router deliver data within a network

### network gateways

- network gateway: device or node that connects networks with different transmission protocols and performs protocol conversions to translate communications
- has a network interface card with inputs, outputs, and software for this translation of network protocols.
- serve as an entry and exit point for a network

## Edge Networking

- infrastructure and software that moves data processing and analysis as close as necessary to where data is created.
- used to deliver intelligent, real-time responsiveness, and streamline the amount of data transferred.
- includes services for caching, storage, databases, compute, etc
- Edge locations: are remote locations outside of a normal cloud/on-premises data center.
  - often lack connectivity or have intermittent connectivity to the cloud.
  - require real-time or low-latency data collection and processing

## Hybrid Networking

- blending of cloud resources with on-premises resources.
- use cases include applications requiring low-latency connectivity to on-premises systems and data residency requirements.
- implemented via a combination of
  - gateways to connect standard on-premises resources to resources in the cloud
  - hardware that moves cloud-native technology to the on-premises location.

### Quality of Service policy (QoS),

- restrict each service or system to a maximum and minimum amount of bandwidth usage.
- its all about enforcing limits on the services utilizing a network to avoid the noisy neighbor problem

## Data driven network design

- understand how networking impacts performance, determine the workload requirements for bandwidth, latency, jitter, and throughput.
  - network latency often impacts the user experience.
  - Not providing enough network capacity can bottleneck workload performance.
- the network is between all application components
  - can have large positive and negative impacts on application performance and behavior.
- When there is a requirement for on premises communication
  - ensure that you have adequate bandwidth for workload performance
  - estimate the bandwidth and latency requirements for your hybrid workload
- Use load balancing and encryption offloading
  - Distribute traffic across multiple resources or services to allow your workload to take advantage of the elasticity that the cloud provides
- Choose network protocols to optimize network traffic
  - There is a relationship between latency and bandwidth to achieve throughput.
- Choose location based on network requirements
  - user location: Choosing a location close to your workload’s users ensures lower latency when they use the workload.
  - data location: For data-heavy applications, the major bottleneck in latency is data transfer. Application code should execute as close to the data as possible.
- Optimize network configuration based on metrics
  - collected and analyzed data to make informed decisions about optimizing your network configuration
  - Measure the impact of those changes and use the impact measurements to make future decisions.

## DNS

- registrant: buy domain names from resellers or registrars
- reseller: third party company (e.g. aws) providing domain name registration services through ICANN accredited registrars
- ICANN: Internet Corporation for Assigned Names and Numbers
- registrar: organization that interfaces between registrant and registry
  - sell domain names, provide registration and other services applicable to domain names
- registry: organization that contains the authoritative name servers and hosted zones of one/more top level domains
  - authority is given to the registry from ICANN

### hosted zones

- a container for records
- records: contain information about routing for a specific domain and its subdomains
- anycast: broadcasts queries to all endpoints
