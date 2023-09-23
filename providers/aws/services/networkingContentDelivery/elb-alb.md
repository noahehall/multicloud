# Application Load Balancer (ALB)

- Load balance HTTP and HTTPS traffic with advanced request routing targeted at the delivery of modern applications.
- layer 7 http/2/s/grpc

## my thoughts

## links

- [landing page](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/?nc=sn&loc=2&dn=2)
- [authnz](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)
- [sticky sessions](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/sticky-sessions.html)

## best practices

### anti patterns

## features

- route traffic based on request data e.g. paths, headers, hosts, etc
- send fixed responses directly to the client
- TLS offloading/termination
- user authnz: uses OpenID Connect (OIDC) and supports SAML, LDAP, microsoft active directory, etc
- rich metrics and logging
- sticky sessions with http cookies to send subsueqent requests to the save backend target

### pricing

## basics

- schemes
  - internet facing: routes public requests
  - internal: routes private ip requests to targets with private ips

### Targets

- types: ip, instance, lambda

#### ips

- allows load balancing to an application backend hosted on any IP address and any interface on an instance.

#### lambda

- invoking Lambda functions to serve HTTP(S) requests

### protocols

#### websockets

#### grpc

### Routing

- redirects: incoming request from one URL to another URL
- enhanced container support by load balancing across multiple ports on a single EC2 instance

#### fixed response

- respond to incoming requests with HTTP error response codes and custom error messages from the load balancer itself, without forwarding the request to the application.

#### Sticky Sessions

- route requests from the same client to the same target.
- support both duration-based cookies and application-based cookies
- enabled at the target group level.

#### Round Robbin

- supports a slow start mode that allows you to add new targets without overwhelming them with a flood of requests
- targets warm up before accepting their fair share of requests based on a ramp-up period
- use cases
  - applications that depend on cache and need a warm-up period before being able to respond to requests with optimal performance.

#### Content Based

- route a request to a service based on the content of the request such as Host field, Path URL, HTTP header, HTTP method, Query string or Source IP address.

##### Host-based

- based on the Host field of the HTTP header allowing you to route to multiple domains from the same load balancer.

##### Path-based Routing

- based on the URL path of the HTTP header.

##### HTTP header-based routing

- based on the value of any standard or custom HTTP header.

##### HTTP method-based routing

- based on any standard or custom HTTP method.

##### Query string parameter-based routing

- based on query string or query parameters.

##### Source IP address CIDR-based routing

- based on source IP address CIDR from where the request originates.

### monitoring

- request tracing: allows you to track a request by its unique ID as it makes its way across various services
  - injects a custom identifier “X-Amzn-Trace-Id” HTTP header on all requests coming into the load balancer.

### security

- supports implementation of Desync protections based on the http_desync_guardian library

#### Encryption

##### TLS Offloading

- supports client TLS session termination
- offload TLS termination tasks to the load balancer, while preserving the source IP address for your back-end applications

#### Server Name Indication

- an extension to the TLS protocol by which a client indicates the hostname to connect to at the start of the TLS handshake
- use SNI to serve multiple secure websites using a single TLS listener.
- If the hostname in the client matches multiple certificates, the load balancer selects the best certificate to use based on a smart selection algorithm.

## considerations

## integrations

### EKS

- managed by Ingress objects
  - used with pods that are deployed to nodes or fargate

### Outposts

### ACM

### ECS

- specify a dynamic port in the ECS task definition, giving the container an unused port when it is scheduled on the EC2 instance.
- The ECS scheduler automatically adds the task to the load balancer using this port.

### WAF

### IAM

### Cognito

- offload the authnz from your apps into Application Load Balancer.
