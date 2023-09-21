# VPN

- Connect your on-premises networks and remote workers to the cloud
- client VPN: fully managed, elastic VPN service that automatically scales up or down based on user demand
- site-to-site VPN: creates a secure connection between your data center or branch office and your AWS cloud resources
- vpn cloudhub: hub & spoke communication between multiple site-to-site locations

## my thoughts

## links

- [landing page](https://aws.amazon.com/vpn/?did=ap_card&trk=ap_card)
- [site to site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)
- [client vpn](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html)
- [cloudhub](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPN_CloudHub.html)

## best practices

### anti patterns

## features

- Securely access your AWS Client VPN with federated and multi-factor authentication (MFA).
- Scale your Client VPN up or down based on user demand with pay-as-you-go pricing.
- Get extensive availability for AWS Site-to-Site VPN with multiple global AWS Availability Zones.
- Accelerate and automatically reroute your Site-to-Site VPN traffic to the nearest and healthiest network endpoint.

### pricing

- site-to-site vpn
  - connection per hour (varies by Region)
  - Data transfer out charges
  - accelerated site-to-site has additional costs
  - client vpn
    - charged for the number of active client connections per hour and the number of subnets associated to Client VPN per hour.

## basics

### Site-to-Site VPN

- connect remote offices to AWS
- Based on IPsec technology: uses a VPN tunnel to pass data from the customer network to or from AWS.
- use public IPv4 addresses and therefore require a public virtual interface to transport traffic over Direct Connect.
- One connection consists of two tunnels.
  - Each tunnel terminates in a different AZon the AWS side, but it must terminate on the same customer gateway on the customer side.
- customer gateway: AWS resource that represents your on-premise gateway device within AWS
  - contains information about the type of routing used by the Site-to-Site VPN, BGP, ASN and other optional configuration information.
- customer gateway device: physical device or software application on your side of the AWS Site-to-Site VPN connection.
- on the Amazon side of the connection, either
  - virtual private gateway: the VPN concentrator
    - only one tunnel out of the pair can be active and carry a maximum of 1.25 Gbps
  - Transit gateway: used to interconnect your VPCs and on-premises networks.
    - both tunnels in the pair can be active and carry an aggregate maximum of 2.5 Gbps
    - Each flow (for example, TCP stream) will still be limited to a maximum of 1.25 Gbps
    - supports equal-cost multi-path routing (ECMP) and multi-exit discriminator (MED) across tunnels in the same and different connection

### Accelerated Site-to-Site VPN

- cannot use accelerated Site-to-Site VPN with a Direct Connect public virtual interface.

### Client VPN

- connect your remote team to onpremise and AWS resources
- Based on OpenVPN technology; access your resources from any location using an OpenVPN-based VPN client.
- client VPN endpoint: controls which networks and resources you can access when you establish a VPN connection.
- vpn client application: the software application that you use to connect to the Client VPN endpoint and establish a secure VPN connection.
- client vpn configuration file: includes information about the Client VPN endpoint and the certificates required to establish a VPN connection.
  - You load this file into your chosen VPN client application.

### CloudHub

## considerations

- supports IPv4/IPv6-Dualstack through separate tunnels for inner traffic
  - IPv6 for outer tunnel connection not supported.
- Site-to-Site VPN
  - does not support Path MTU Discovery
  - The greatest Maximum Transmission Unit (MTU) available on the inside tunnel interface is 1,399 bytes.
  - Throughput is limited
  - Maximum packets per second (PPS) per VPN tunnel is 140,000.
- client VPN
  - IPv6 is not supported.
  - SAML 2.0-based federated authentication only works with an AWS provided client v1.2.0 or later.
  - Client VPN endpoint does not support subnet associations in a dedicated tenancy VPC.
  - is not compliant with Federal Information Processing Standards (FIPS).
  - subnets associated with a Client VPN endpoint must be in the same VPC.
  - Client CIDR ranges
    - must have a block size of at least /22 and must not be greater than /12.
    - cannot be changed after you create the Client VPN endpoint.
    - cannot overlap with
      - the local CIDR of the VPC in which the associated subnet is located
      - any routes manually added to the Client VPN endpoint's route table.
  - ACM certificates are not supported with mutual authentication because you cannot extract the private key
    - but you can use an ACM server as the server-side certificate.

## integrations
