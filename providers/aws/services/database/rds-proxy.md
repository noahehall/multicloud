# RDS Proxy

- fully managed, highly available database proxy for Amazon Relational Database Service (RDS) that makes applications more scalable, more resilient to database failures, and more secure.
- completely serverless and scales automatically to accommodate your workload.

## my thoughts

## links

- [landing page](https://aws.amazon.com/rds/proxy/)

## best practices

### anti patterns

## features

- maintains a pool of established connections to your RDS database instances
- shares infrequently used database connections, so that fewer connections access the RDS database
- minimizes application disruption from outages affecting the availability of your database by automatically connecting to a new database instance while preserving application connections
- gives you additional control over data security by giving you the choice to enforce IAM authentication for database access and avoid hard coding database credentials into application code
- centrally manage database credentials using AWS Secrets Manager.
- the benefits of a database proxy without requiring additional burden of patching and managing your own proxy server
- fully compatible with the protocols of supported database engines: point your application connections to the proxy instead of the RDS database
- highly available and deployed over multiple Availability Zones (AZs)

### pricing

- priced based on the capacity of underlying instances
- provisioned instances on Amazon Aurora, RDS for MariaDB, RDS for MySQL, and RDS for PostgreSQL,
  - priced per vCPU per hour
- Aurora Serverless v2,
  - priced per Aurora Capacity Unit (ACU) per hour consumed by your database.

## basics

## considerations

## integrations
