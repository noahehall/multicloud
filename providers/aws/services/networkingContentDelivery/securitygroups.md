# Security groups

- stateful firewall at the resource level: you enable ingress traffic and egress is autoamatically allowed
- resource security at the port, protocol and ip range

## links

- [vpc: intro](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)

## basics

- enable one/more port & ip range per security group
- cannot explicit deny IP address, all ingress is denied by default
  - This means that anything that is not explicitly allowed is denied.
- rules are processed all at once, there is no order
- attached to the elastic network interface of resources in a subnet.
- security groups vs subnet NACLs
  - nacls: applied at VPC subnet
    - stateless: define both ingress and egress at the subnet level
    - only traffic crossing a subnet boundary is filtered
  - security groups: applied at resources
    - stateful: define ingress
    - all traffic entering the resource is filtered

### security group types

#### EC2

- attaches to ec2 instances

#### Database

- attaches to rds/etc instance

#### VPC

- attaches to resources within a VPC

### OSI Model

- operates at layer 3 and 4
- traffic is filtered based on ip addrs, transport protocols and ports

### Statefulness

- Stateful firewalls view traffic as one stream. If traffic is allowed in, then that traffic is automatically allowed back out.
- security groups recognize AWS resources. So you can add rules for these.

#### inbound rules

- type: the protocol
- source: can grant access to
  - a specific CIDR range,
  - another security group in your VPC or in a peer VPC (requires a VPC peering connection)
    - traffic is allowed from the network interfaces that are associated with the source security group for the specified protocol and port
    - Incoming traffic is allowed based on the private IP addresses of the network interfaces that are associated with the source security group
      - not the public IP or Elastic IP addresses
  - etc

## integrations

### ec2

- acts as a virtual firewall for your instance to control inbound and outbound traffic
- security groups are required for EC2 instances
  - can assign up to 5
-

### vpc

- every VPC has a default security group

### RDS

- control which IP ranges or EC2 instances can connect to a db and ec2 instances
- uses 3 types of security groups
  - VPC
  - database
  - EC2

### ELB

- every application load balancer requiests at least one security group
  - else the VPCs default security group will be used
