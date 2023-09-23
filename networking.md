# networking

- TODOs
  - theres a copypasta section you need to deal with at the very bottom

## links

- [Address Allocation for Private Internets](http://www.faqs.org/rfcs/rfc1918.html)
- [networking: intro](https://web.stanford.edu/class/cs101/network-1-introduction.html)
- [ip address & cidr range visualizer](https://cidr.xyz/)
- [understanding ip addressing](https://www.ripe.net/about-us/press-centre/understanding-ip-addressing)

### tools

- [bpftrace: inspect syscalls](https://github.com/iovisor/bpftrace)

## basics

- CSMA: carrier-sense multiple access
- Software Defined Networks (SDN)
  - an approach to networking that uses software based controllers/APIs to communicate with underlying infrastructure and direct traffic
  - a group of software services working together to create a network construct
- Network design documents
  - documents and diagrams visualize the components of a network, including routers, firewalls, and devices
  - show how those components interaca
- http requests
  - method: aka verb; the action that the user agent wants the server to perform
    - GET: fetch
    - POST: create/update
    - PUT: update/upload
    - PATCH: edit
    - DELETE: delete
    - HEAD: retrieves same info as GET, but instructs the server to return the response without a body
    - CONNECT: initiates two-way comms; e.g. connecting through a proxy
    - OPTIONS: lets a user agent ask what other methods are supported by a resource
    - TRACE: will contain an exact copy of the original HTTP request, for the user agent to see what (if any) alterations were made by intermediate servers
  - URL: universal resource locator: describes the resource being manipulated/fetched
  - Headers: metadata; e.g. type of content the user agent is expecting/whether it accepts compressed responses
  - Body: optional component contains any extra data that needs to be sent to the server
- HTTP responses
  - protocol:
  - code: 3 digit status code
    - 2xx: understood, accepted, and responded to
    - 3xx: redirect
    - 4xx: client error; user agent generated an invalid request
    - 5xx: server error; request was valid, but the server was unable to fullfil the request
  - msg: status msg
  - headers: instruct the user agent how to treat the content
    - content-type
    - cache-control
  - body: if a resource was requested
- stateful connections:
  - when a client and server perform a handhsake and continue to send packets back n fourth until one of the communicate parties decides to terminate

### servers

- web servers: computer program (e.g. HAproxy) that validates & routes HTTP requests for dynamic content to application servers, responds directly with static content, and performs low-level TCP functions like HTTPS termination
  - all HTTP traffic should be rerouted to HTTPS
  - web server handling HTTPS should terminate (strip, decrypt) the request before proxying the request to application servers
  - application servers should not be reachable by the public
  - the application server will fullfil the request, and reply to the web server with the content, and the web server will forward the content back to the user agent that made the request
- application server: computer program (e.g. nodejs) that hosts application code, and responds to HTTP requests from web servers, generally handles all requests for dynamic http content
- CDN: content delivery network
  - will store duplicated copies of static resources in data centers around the world
  - enables prouction of responsive websites without a massive server expenditure
  - security issues:
    - allows a third party to serve content under your security certicate
- CMS: content management systems
  - provide authoring tools requiring little/no technial knowedlge to wriet content
  - cms plugins provide additional tooling, e.g. anlytics
  - security issues
    - using a cms/plugins makes you more secure if you utilize high fidelity packages from reputable vendors
    - but also makes them a high profile target for hackers, e.g. wordpress is always getting fkn hacked
- http session: the entire conversation (stateless/stateful) between a specific user agent & server
  - server could send a set-cookie header in the initial HTTP response containing data that identifies the user agent
    - the user agent will store & send back the same cookie on each subsequent response
- resources
  - static: an object thats returned unaltered in HTTP responses
  - dynamic: an object thats executed/interpreted based on data in HTTP requests and computed before returned in HTTP responses
    - often the code loads data from a database in order to populate the http response
    - security issues
      - the dynamic interpolation of content can be vulnerable to attack
- URL resolution
  - enable any URL to be mapped to a particular static resource
  - by unlinking the URL from a filepath, you have more freedom in organizing your code
    - e.g. having each user have a different profile image on disk, but using the same URL path /user/profile/image

### user agents

- web browsers
  - javascript engine
  - rendering pipeline
  - connect with operating system to resolve and cache DNS addresses
  - interpret and verify security certificates
  - encode requests in HTTPS
  - store and transmit cookies according to the web servers instructions
  - browser security model
    - dictates
      - js code must be executed within a sandbox,
        - disabling the following actions
          - start new processes/access existing process
          - read arbitrary chunks of system memory
          - access the local disk
          - access the operating systems network layer
          - call operating system functions
        - enabling the following actions
          - read & manipulate the DOM of the current page
          - listen & respond to user actions via event listeners
          - make http calls on behalf of the user
          - open new webpages/refresh the URL of the current page ONLY in response to user actions
          - write new & navigate between entries in the browser history
          - ask for users location
          - ask permission to send desktop notifications
    - rendering pipeline: software component within a web browser responsible for transforming HTML into its visual representation
      - parse the HTML
        - tokenize
      - generate the DOM
        - an in-memory data structure that represents the browsers understanding of how the page is structured,
          - a series of nested elements called DOM nodes, each roughly equivalent to an HTML tag
        - parse the HTML into a DOM
          - whenever an external resource is encountered, stop an retrieve it
            - i.e. script, style, image, font, video, etc tags all stop the rendering pipeline to retrieve the external thing
          - script tags
            - ensure the `defer` attribute is added so the script tag doesnt execute until the rendering pipeline is completed
      - generate the CSSOM
        - styling rules applied to each DOM element
          - which correspond to onscreen elemnts
          - how to paint each element relative to eachother
          - what styling to apply to each
      - DRAW/PAINT
        - draws the webpage on screen
      - EXECUTE JS
        - this step is actually interwoven between generation of the DOM and DRAW
        - the browser will load & execute any JS it comes across as it constructs the DOM
        - and the JS can dynamically make changes to the DOM and styling rules, either before the page is rendered or in response to user actions

### sessions

- session: HTTP conversation in which the browser sends a series of HTTP requests corresponding to a specific entity (e.g. a user), and the web server recognizes them as corresponding to the same entity; the initial request is usually tagged with an ID, and that ID is sent back in the response
- session ID: typically a large, randomly generated number: the minimal information the browser needs to transmit with each subsequent HTTP request so the server can continue the HTTP conversation from the previous request
  - remember, these are generally just random integers
  - can be transmittd via URL, http header, body of requests
  - but best practice is to send as a session cookie via the `Set-Cookie` header of the http response
    - the browser will natively send this cookie & value back on subsequent requests automatically to the server that set it
- server side sessions: the web server keeps the session state in locally/remotely (e.g. in file/cache/db/etc), and both the server & user agent pass the session ID back n forth
  - the server stores & retrieves other session state data in a remote/local DB/cache/file of some sort
  - scalability issues: requires that backend servers have access to other servers session information (e.g. in a load balanced architecture)
- client side sessions: web servers send the serializes entire session state (and not just the session ID) in the cookie, this alleviates the need to share session state amongst BFF servers
  - security issues: attackers can manipulate/forge the data stored in the cookie and your BFFs will be none the wiser

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

### Quality of Service policy (QoS)

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

# copypasta from security file

#### internet protocol suite

- internet protocol suite: dictates how computers exchange data over the web
  - there are over 20 protocols collectively under this umbrella
- internet protocol layers
  - network layer
    - ARP
    - MAC
    - NDP
    - OSPF
    - PPP
  - internet layer
    - IPv4
    - IPv6
  - transport layer
    - TCP
    - UDP
  - application Layer
    - TLS
    - SSL
    - SSL
    - DNS
    - FTP
    - HTTP
    - IMAP
    - POP
    - SMTP
    - SSH
    - XMPP

##### internet layer

- IP: internet protocol addresses
  - destination for data packets
  - unique binary numbers assigned to individual internet-connected computers
  - IPv4: 2x32 addresses
  - IPv6: represented as 8 groups of 4 hexadecimal digits separated by colons

##### Transport Layer Protocols

- TCP: transmission control protocol
  - enables two computers to reliably exchange data over the internet
  - created in response to ARPANET (predecessor to the internet)
  - the first msg sent (was on ARPANET) was a LOGIN command destined for a remote computer at stanford university, but crashed after the first two letters (reason for TCP)
  - high level workflow
    - messages sent via TCP are split into data packets
    - the servers that make up the internet push these packets from sender to receiver without having to read the entire msg
    - the receiver reassembles all the data packets into a usable order according to the sequence number on each packet
      - each packet the receiver gets, it responds with a receipt back to the sender
      - without the receipt, the sender will resend the packet
        - possibly along a different network path
        - possibly at an adjusted speed based on the speed of consumption by receiver
    - this send & receipt workflow guarantees msg delivery
    - TCP doesnt dictate how the data being sent is meant to be interpreted, that occurs at a higher level protocol (e.g. HTTP)
      - unencrypted TCP data are vulnerable to man in the middle attacks, see TLS for more info
- UDP: User Datagram Protocol
  - newer than TCP
  - commonly used with video/situations where dropped data packets are expected/msg guarantee isnt required, but the data packets can be streamed at a constant rate

##### Application Layer Protocols

- TLS: transport layer security
  - arguable what fkn layer this is actually in (some say its not the application layer, but a lower layer)
    - makes sense it would be in the transport layer, (because of the name)
  - method of encryption that provides both privacy and data integrity
  - ensures that
    - privacy: packets intercepted by a third party cant be decrypted without the appropriate encryption keys
    - data integrity: any attempt to tamper with the packets will be detectable
  - workflow
    - HTTPS (http secure) requires the client & server to perform a TLS handshake
      - both parties agree on an encyption method (cipher) and exchange encryption keys
    - any subsequent data packets (request & responses) will be opaque to outsiders
  - TLS Handshake: consists of assymetric and symmetric encryption, and Message Authentication Codes for fingerprinting (see cipher suites)
    - selection of the cipher used for encryption & decrypting all data packets
      - user agents will inform servers which cipher suites it supports
      - and the server replies with the best cipher suite that it also supports, the servers digital certificate and the encryption (public) key
      - the user agent verifies the authenticity of the certificate with the issueing certificate authority
      - the user agent generates a session key, encrypts it with the servers public key using the key-exchange algorithm from the chosen cipher suite and sends it to the server
        - the session key (another large random integer) is used to encrypt all subsequent TLS conversation (data packets) with the block cipher from the cipher suite chosen by the server
      - now data packets can finally be sent over TLS efficiently using the symmetric block cipher
      - additional info
        - the initial phase is to use assymetric encryption to encrypt the block cipher key before passing it to the recipient
        - this is to prevent theft of the single key used to encrypt & decrypt symmetrically encrypted data
        - block ciphers: most data packets will be symmetrically encrypted for efficiency, the recipient should already have the encryption & decryption key from the first phase of the handshake
        - the block ciphers are also tagged with a MAC; so both parties can authenticate messages & detect if ANY packets have been tampered with (data integrity)
  - cipher suites: each suite is a set of 3 algorithms used to secure communication; always use the latest TLS cipher suite
    - key-exchange algorithm: the first algorithm; assymetric; used by communicating computers to exchange secret keys
    - symmetric block cipher: the second algorithm; used for encrypting the content of TCP packets
    - MAC algorithm: for authenticating the encrypting messages havent been tampered with
    - e.g. TLS 1.3 offers numerous cipher suites, one of them being ECDHE-ECDSA-AES128-GCM-SHA256
      - ECDHE-RSA: the key exchange algorithm
      - AES-128-GCM: the block cipher
      - SHA-256: the message authentication algorithm
  - digital certificates: aka public-key certificate; an electronic document issued by third-party certificate authorities to prove which internet domain owns which public encryption key
    - contains: server domain name, the issueing certificate authority, an encryption public key
    - that way user agents can confirm the server (some IP) they are communicating with is valid for this domain (e.g. google.com) and this certificate
    - that way an attacker cant impersonate a domain or a certifcate the UA checks with the certificate authority in the initial phases of the TLS handshake
    - self signed certificates: digital certs not issued by a certificate authority; useful for internal domains and development environments
    - Certificate Signing Request: CSR; contain info about the applicant & domain that is all useful in verifying authenticity; often created with openssl on the cli
      - domain name: distinguished name (DN) or the fully qualified domain name (FWDN)
      - organizations legal name
      - physical location
    - domain verification: process by which a ceritficate authority verifies that someone applying for a certificate for an internet domain does indeed have control of that domain
      - domain verification is what protects against DNS spoofing attacks; an attacker cnanot apply for a cerificate unless they also have DNS access rights to that domain
      - Extended validation (EV) certificates: require the certificate authority to collect and verify information about hte legal entity applying for a certificate; popular with large organizations because the name of the org is often displayed alongside the padlock in the browser url
      - certificates have a finite lifespan (years/months) and can be voluntarily revoked by the owner
    - general process: is all about having the certificate authority verify ownership of a particular domain, and then giving you a certificate that can be used to decrypt traffic sent to that domain,
      - generate a key pair: digital file containing randomly generated public and private encryption keys
      - use the key pair to generate a Certificate Signing Request (CSR) that contains the pulic key and domain your requesting the certificate for
      - upload the CSR to the certificate authority, and the cert authority will then require you to validate ownership by making some DNS change with values they specify
      - once ownership is proven: you will be given a digital cert for use on your domain server along with the key pair previously created
  - HTTP Strict Transport Security: HSTS; policy that ensures sensitive data (e.g. cookies) will not be sent during any initial connection over HTTP, and must wait for the TLS handshake to be completed
    - when a user agent visits a site it has seen previously, it will automatically send back any cookies the website previously supplied in the Cookie header
    - if the initial connection was insecure, then the cookies will be sent back insecurely, even if subseqent requests were handled over HTTPS
- SMTP: simple mail transport protocol
  - for sending emails
- XMPP: extensible messaging and presence protocol
  - instant messaging
- FTP: file transfer protocol
  - downloading files from servers
- HTTP: hypertext transfer protocol
  - transport webpages and their resources to user agents like web browsers
  - workflow: general
    - user agents generate requests for specific resources
    - web servers expecting those requests, return responses containing either the requested resource, or an error code
    - both requests & responses are plain text msgs, but can be delivered as compressed &/ encrypted
    - the majority of web exploits use http in some fashion
  - authentication: the process of identifyng users when they return to your application
    - http native authentication is rarely used since you cant customize the login form presented by the browser
      - to present an authentication challenge, a web server returns a 401 status code in the HTTP respone and adds a `WWW-Authneticate` header describing the preferred authentication method
      - basic authentication scheme:
        - the user agent (e.g. a browser) requests a username & password from the user
        - the browser concatenates the username + password separeted by a colon, e.g. `myname:mypw`
        - uses the base64 algorithm to encode this strng and sends it back to te server in the `Authorization` header of the http request
      - Digest authentication scheme:
        - requires the browser to generate a hash consisting of the username, password, and URL
    - non-native authentication: generally presented through a custo HTML form, whose action is to POST to some bff
- DNS: domain name system
  - a global directory that translated IP addrs to unique human readable domains e.g. nirv.ai
  - domain registrars: private organizations that register domains before they can be used in DNS
  - workflow
    - when a browser encounters a domain for the first time
      - check the local domain name server (typically hosted by an ISP) to get the associated IP (and various other data) and cache the result
  - terms
    - TTL: time to live: how long a domain name server will cache the IP addr associated with a domain
      - i.e. DNS caching
    - CNAME: canonical name records
      - i.e. aliases for domain names
      - enable multiple domain names to point to the same IP address
    - MX: mail exchange records
      - help route email
