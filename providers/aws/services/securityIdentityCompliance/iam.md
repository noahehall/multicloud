# Identity and Access Management (IAM)

- centralize privilege management using the principle of least privilege, separation of duties and temporary credentials
- authnz for:
  - IAM users and groups: humans logging into an account and signing API calls
  - IAM roles: assumed by an entity (humans/machines) for temporary access to AWS credentials
- theres a lot of crossover in the STS file

## my thoughts

- everything starts and ends with IAM
  - most setups should also use AWS Organizations

## links

- [security credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html)
- [faqs](https://aws.amazon.com/iam/faqs/?da=sec&sec=prep)

### user guide

- [AAA security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AAA: getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html)
- [access history: finding unused credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_finding-unused.html)
- [access history: tightenting s3 permissions](https://aws.amazon.com/blogs/security/tighten-s3-permissions-iam-users-and-roles-using-access-history-s3-actions/)
- [apigateway: authnz workflow](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-authorization-flow.html)
- [apigateway: Identity Based policy examples](https://docs.aws.amazon.com/apigateway/latest/developerguide/security_iam_id-based-policy-examples.html)
- [apigateway: resource policies](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-resource-policies.html)
- [authnz for resources](https://docs.aws.amazon.com/en_us/IAM/latest/UserGuide/access.html)
- [dynamodb: intro](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/UsingIAMWithDDB.html)
- [ec2: iam roles](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
- [ec2: iam](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-iam.html)
- [ecs: task roles](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)
- [enabling mfa](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)
- [groups and jobs functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html)
- [identities: roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [identities: users groups and roles](https://docs.aws.amazon.com/en_us/IAM/latest/UserGuide/id.html)
- [identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html)
- [intro to IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html)
- [mfa](https://aws.amazon.com/iam/details/mfa/)
- [neptune: iam auth](https://docs.aws.amazon.com/neptune/latest/userguide/iam-auth.html)
- [neptune: iam policy page](https://docs.aws.amazon.com/neptune/latest/userguide/security-iam-access-manage.html)
- [organizations: SCPs > AAA landing page](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
- [organizations: SCPs > evaluation](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_evaluation.html)
- [organizations: SCPs > examples](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html)
- [organizations: SCPs > inheritance](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_inheritance_auth.html)
- [organizations: SCPs > intro BLOG](https://aws.amazon.com/blogs/security/how-to-use-service-control-policies-in-aws-organizations/)
- [organizations: SCPs > vpc sharing BLOG](https://aws.amazon.com/blogs/security/control-vpc-sharing-in-an-aws-multi-account-setup-with-service-control-policies/)
- [policies: access](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
- [roles: service linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)
- [signing aws api requests (sig v4)](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-signing.html)
- [ssm: iam](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-iam.html)
- [ssm: profile](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-instance-profile.html)
- [ssm: service linked roles](https://docs.aws.amazon.com/systems-manager/latest/userguide/using-service-linked-roles.html)
- [step functions: advanced permissions](https://docs.aws.amazon.com/step-functions/latest/dg/concept-create-iam-advanced.html)
- [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html)
- [testing iam policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
- [tutorial: ABAC via tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_attribute-based-access-control.html)
- [vpc: reachability analyzer perms](https://docs.aws.amazon.com/vpc/latest/reachability/security_iam_required-API-permissions.html)

### API

- [root user only tasks](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)
- [lambda: resource based policies](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)

### tools

- [iam policy simulator](https://policysim.aws.amazon.com/)

## best practices

- use the fkn policy simulator, it should be your best friend
- resource policies are easier to grant/deny access across services/accounts, but have size limits
  - iam roles are a bit more verbose but dont have limits
- users
  - protect the root user at all costs
    - create and use an admin user instead of the root account
  - always enable multi factor auth
  - manage users through Identity Center and not the IAM console
    - you can set AWS as the Idp
- groups
  - always assign users to groups, and attach policies to groups (and not directly to users)
  - should reflect organizational role, not technical commonality
- policies
  - always follow principle of least privilege for users, groups and roles
  - generally the managed policies are too lenient and should be customized
- roles
  - use both RBAC and ABAC at the same time if using IAM roles for access control.
- organizations that spans multiple AWS accounts;
  - manage access to all the AWS accounts centrally via identity federation because users and groups are not scalable.
  - Using roles and cross-account access
    - define user identities in one account and use those identities to access AWS resources in other accounts that belong to your organization.

### anti patterns

## features

- Set and manage guardrails and fine-grained access controls for your workforce and workloads.
- Manage identities across single AWS accounts or centrally connect identities to multiple AWS accounts.
- Grant temporary security credentials for workloads that access your AWS resources.
- analyze access to right-size permissions on the journey to least privilege.

## pricing

## basics

- access in AWS is managed by creating policies and attaching them to IAM identities (users, groups of users, or roles) or AWS resources.
- IAM acts as an identity provider (IdP), and manages identities inside an AWS account

### authentication schemes

- uname & pword: for accessing the console
  - define a password policy to enforce strong passwords and to require password rotation
- access keys: for programmatic access; caccess key ID and a secret key; Each user can have two active access keys
  - cli access
  - local code in a dev env to access AWS account
  - apps running on compute services
  - third-party services to access the aws account
  - direct http calls using APIs for individual services
  - apps run outside of AWS
- mfa: via soft/hardware; requires an additional input to validate a login attempt
  - something you know: e.g. a pin number
  - something you have: e.g. a onetime code from an app/device
    - virtual MFA: softare
    - hardware TOTP token
    - FIDO security keys
  - something you are: e.g. fingerprint or piece of your soul

#### request signatures

- signing a request enables AWS to authenticate your identity
- users and groups use the credentials associated with their acounts
- machines (e.g. any of your aws services) must assume a predefined role and sign requests with temporary credentials

### users & groups

- for managing access to resources WITHIN your account
  - use federated identities to centrally managed acess ACROSS accounts
- root user: the initial user on the aws account
- non root users: represent people and also applications that need access to an AWS account.
- groups: are collections of users; specify permissions for similar types of users
- characteristics
  - static credentials
  - dont expire by default but you should definitely set requiremnets for periodic rotation

#### Federated Identities

- for identities stored outside AWS.
- a system of trust between two parties for the purpose of authenticating users and conveying information needed to authorize their access to resources
- identity provider: (IdP) is responsible for user authentication
  - the metadata passed from IdP to AWS can be used in ABAC
- service provider: such as a service or an application, controls access to resources
- logical workflow
  - a trust relationship is configured between an IdP and a service provider
    - the service provider relies on the IdP for authentication and user information for authorization
  - after authenticating a user, the IdP returns an `assertion` containing the users data
  - the service provider uses the assertion data for creating a session and determining the scope of authorization within its service boundary
    - the service provider provides the user with credentials

##### federation protocols

- SAML 2.0: Security Assertion Markup Language
- OIDC: OpenID Connect
- OAuth 2.0

##### corporate identity federation

- workforce federation: employees, contractors and partners
- generally relies on SAML and STS AssumeRoleWithSAML

##### web identity federation

- end user federation: customer facing web/mobile applications
- generally relies on OIDC/OAuth and STS AssumeRoleWithWebIdentity
- web identity providers supported by AWS include Amazon Cognito, Login with Amazon, Facebook, Google, or any OpenID Connect-compatible identity provider.

### roles

- delegate access to users, applications, or services
- endow an entity with temporary credentials to perform some function
  - users and groups
  - machines: for service-to-service authnz
  - federated users: external identity providers
    - SAML 2.0 federation
    - web identity
  - custom trust policy
- characteristics
  - the entity that assumes the role will lose its original privileges and gain the access associated with the role
  - no static credentials: must be progrogrammatical requested
  - credentials are always temporary for the requested amount of time
- execution role: enables a service to assume some role with some predefined behavior for interacting with other services
  - determines what service A can do within a service boundary
- passing roles: the user passing the role must have permissions to pass the role
  - ensure that only approved users can configure a service with a role that grants permissions
- check the STS file as well

#### role chaining

- occurs when you use a role to assume a second role through the AWS CLI or API
- assume one role and then use temporary STS creds to assume another role and continue from session to session

##### Transitive Tags

- tags that persist through multiple sessions, e.g. when role chaining
- use cases
  - impose guardrails against yourself or an administrator in order to prevent something accidental
    - e.g. require an admin role to assume a lesser privileged role; instead of creating a totally new admin role

#### service roles

- iam roles that can be assumed by an AWS service
- Service roles enable other AWS services to integrate with each other
- must include a trust policy
  - or (i think) permission to sts->assumeRole

##### service linked roles

- type of service role that is linked to a specific aws service
- appear in your in AWS account and are owned by the service
- you can view but not edit service linked roles

### policies

- [policy reference](./iam-policy-ref.md)
- set of permissions that grant/deny users, groups and roles to invoke resource actions
  - anything not explicitly granted is denied by default
- there are 6 policy types that fall into two policy categories (grants & guardrails)
  - all are evaluated before any request is allowed/denied

#### Grants

- policies that are primarily used to grant access

##### Identity Based Policy (aka iam policies)

- policies attached to an Identiy that Impacts IAM principal permissions
  - identity policies do not have a principal element because they are directly attached to the principal
- identity: user, group or role
- types
  - AWS managed
    - default policies created and managed by AWS
    - recommended for new users
  - Customer Managed
    - user created and enables precise control over permissions applied to entities
    - should be preferred over Managed policies
  - inlined
    - strict one-to-one relationship between a service and principal
    - embedded directly into a single user, group or role
    - not recommended
- size restrictions: for all policies attached to an entity
  - User 2kb
  - Role 10kb.
  - Group 5kb.

##### resource based policies

- inline policies that are attached to AWS resources that grant/deny identity/accounts
  - resource policies will ALWAYS have a principal element
- determines who is allowed into a service boundary
- trump identity based policies

###### role Trust Policies

- controls which principals in other accounts can access resources
- resource based policies that are attached to a role that define which principals can assume the role

##### Access Control Lists (ACLs)

- control which principals in other accounts can access the resources to which the ACL is attached
  - cross-account permissions policies that grant permissions to the specified principal
  - ACLs cannot grant permissions to entities within the same account.
  - supported by Amazon S3 buckets and objects.
- similar to resource based policies although they are the only policy type that does not use the JSON policy document structure

#### Guardrails

- policies that are primarily used to restrict access

##### Permissions Boundaries

- sets the maximum permissions that an Identity Based policy can grant to an IAM identity
- The entity can perform only the actions that are allowed by BOTH its identity-based policies and its permissions boundaries
  - i.e. if there is no explicit allow in the permissions boundary, then its evaluated as an implicity deny
    - the implicit deny WILL TRUMP an identity based policy's explicit allow

##### Service Control Policies (SCPs) for AWS Organizations

- see the organizations section under integrations

##### Session Policies

- inline permissions policy that creates a session policy when assuming a role
  - by default: all users assuming the same role get the same permissions for their role session
  - this allows you to create distinctive role session permissions to further restrict overall permissions
    - Restricts/limits permissions for assumed roles and federated users
    - Reduce the number of roles they need to create because multiple users can assume the same role yet have unique session permissions.
    - Set permissions for users to perform only those specific actions for that session
- any of the three AssumeRole APIs can be used to pass the session policies.
- with Identity Based policies
  - the effective permissions are the intersection of the IAM entity's Identity Based policy and the session policies
- with resource based policies:
  - specify the ARN of the user or role as a principal
  - The session policy limits the total permissions granted by the resource based policy and the Identity Based policy
  - The effective permissions are the intersection of the session policies and either the resource based policy or the Identity Based policy.

###### session names

- always provide a role session name to:
  - uniquely identify users
  - target users/apply policies via condition keys,
    - e.g. always requiring a role session name that matches the user name
  - track users via cloudtrail logs
- depends on the method used to assume a role
  - aws service:
    - AWS may set the role session name on your behalf
  - SAML based:
    - AssumeRolewithSAML API: AWS sets the role session name value to the attribute provided by the identity provider
  - user defined: you provide the role session name when assuming the IAM role
    - assuming an IAM role with APIs such as AssumeRole or AssumeRoleWithWebIdentity
      - the role session name is a required input

###### Session Tags

- attributes passed in an IAM role session when you assume a role or federate a user using the AWS CLI or AWS API
  - Session tags are principal tags that you specify while requesting a session.
- use cases
  - access control in IAM policies
  - use SAML attributes for access control in AWS (e.g. for federated users)
  - monitoring: view the principal tags for your session, including its session tags, in the AWS CloudTrail logs.
  - control the tags that can be passed into a subsequent session.
  - define unique permissions based on user attributes without having to create and manage multiple roles and policies
- requirements
  - you must have the sts:TagSession action allowed in your IAM policy
  - must follow the rules for naming tags in IAM and AWS STS
  - New session tags override existing assumed role or federated user tags with the same tag key, regardless of case.
  - cannot pass session tags using the AWS Management Console.
  - valid for only the current session and are not stored in AWS (only in the session)
  - pass a maximum of 50 session tags.

#### policy evaluation order

- IAM authenticates the principal
- IAM validates the principals authorization against the requested actions
  - all attached Identity Based permissions
  - all resource based policies attached to the resource
- IAM evaluates allow/deny rules: explicity denies override explicit allows
  - deny by default if no explicit allow/deny
  - allow if explicitly allowed and no explicit denial
    - a permissions boundary, AWS Organizations SCP, or session policy can implicitly deny and override any explicit allows
  - deny if explicitly denied
- internal vs external accounts: need a service control policy
  - within an account: AND an IAM policy OR a resource based policy
  - Across accounts: AND an IAM policy AND a resource based policy.

#### policy simulator

- test and troubleshoot Identity Based policies, IAM permissions boundaries, AWS Organizations service control policies, and resource based policies
  - evaluates the policies that you choose and determines the effective permissions for each of the actions that you specify
- does not make an actual AWS service request, so no charges are occurred
  - merely tests if the request would be accepted/rejected
- use cases
  - Test policies that are attached to IAM users, groups, or roles in your AWS account
    - test which actions are allowed or denied by the selected policies for specific resources.
  - Test and troubleshoot the effect of permissions boundaries on IAM entities one permissions boundary at a time.
  - Test policies that are attached to AWS resources, such as Amazon S3 buckets, Amazon Simple Queue Service (Amazon SQS) queues, Amazon Simple Notification Service (Amazon SNS) topics, or Amazon S3 Glacier vaults.
  - Test the impact of SCPs on your IAM policies and resource policies if your AWS account is a member of an organization in AWS Organizations.
  - Simulate real-world scenarios by providing context keys, such as an IP address or date, that are included in Condition elements in the policies being tested.
  - Identify which specific statement in a policy results in allowing or denying access to a particular resource or action.

### Request Context

- what is being authenticated and authorized by IAM
- principal: a subject that can make a request for an action/operation on an AWS resource
  - aws accounts: delegate authority to the account; the permission policies can be scoped to specific account identities
  - iam users
  - federated users
  - iam roles
  - aws services
- actions: verb; What the principal is attempting to do
  - via console its known as an ACTION
  - via cli/sdk its known as an OPERATION
- resource: object; AWS resource object upon which the actions or operations are performedf

#### authorization context

- an array of key values that scope principal, actions and resources
- this is the environment in which the request is being made, contains bunches of things

### Access Points

- every action taken against an AWS account occurs through an API endpoint
- this allows for comprehensive monitoring and control of in/outbound network traffic
- No matter the means used to access AWS
  - each API request is authenticated and authorized via IAM
  - recorded by AWS CloudTrail as it crosses the AWS API interface
  - All of the API endpoints support HTTPS and use TLS encryption

### Access Control

- most versatile strategy is to use both RBAC + ABAC
  - roles split out users by functions
  - attributes split out roles by context
    - reduces the number of roles required, as a role can be segmented based on the context of user attributes

#### Tags

- Tags enable customizable key-value pairs, such as a project name or an environment type, to identify IAM principals and AWS resources.
- fundamental to ABAC

#### RBAC

- implemented by creating policies for each job function.
- Identities such as IAM users, groups of users, or an application can assume these policies via roles

#### ABAC

- implemented via tags attached to users, roles and resources
- policies can be designed to allow operations when the principal's tag matches the resource tag

#### cross-account delegation

- a trust policy must first exist between the two accounts
  - trusting account owns the resource to be accessed
  - trusted account contains the users who need access to the resource
- create one set of long-term credentials in one account.
  - Then, you use temporary security credentials to access all the other accounts by assuming roles in those accounts.

### Access History

- view last accessed information for entities or policies that exist in IAM or AWS Organizations.
- IAM provides last accessed information to help you:
  - identify unused permissions
  - refine your policies
  - allow access to only the services and actions that your IAM entities use
- last access information types
  - allowed AWS service information
  - allowed action information
- last access information by resource
  - user: last time that the user tried to access each allowed service.
  - group: last time that a group member attempted to access each allowed service.
    - This report also includes the total number of members that attempted access.
  - Role: last time that someone used the role in an attempt to access each allowed service.
  - Policy: last time that a user or role attempted to access each allowed service.
    - This report also includes the total number of entities that attempted access.

#### Access Advisor tab

- in console: IAM dashboard > iam resource > resource > Access Advisor
  - can also be access via cli/sdk
- considerations
  - tracking period: data appears within 4 hours, and kept for 400 days
  - PassRole: iam:PassRole action is not tracked and is not included in IAM service last accessed information.
  - report owner: Only the principal that generates a report can view the report details
    - when using the cli/sdk, you must use the report principals' creds
    - If you use temporary credentials for a role or federated user, you must generate and retrieve the report during the same session.
  - Entities:
    - IAM: includes IAM entities users or roles in your account
    - organizations: includes IAM users, IAM roles, or the AWS account root user in the specified Organizations entity
    - does not include unauthenticated attempts.
  - IAM Policy types: includes services that are allowed by an IAM entity's policies
    - policies either attached to a role or attached to a user directly or through a group
      - Access allowed by other policy types is not included in your report
      - not included: resource based policies, access control lists, AWS Organizations SCPs, IAM permissions boundaries, and session policies.
  - required permissions: are different when accessed via cli & console
- use cases
  - reducing permissions for user/groups: see what they've accessed, and restrict their actions
  - deleting IAM resources: ensuring a time has passed since the last access before deleting
  - editing/detaching policies: see which users are using which policies before modifying

## integrations

- basically every other AWS service uses IAM for authNZ

### organizations

- policies specific to AWS organizations

#### Policies

- central control over the maximum available permissions for all accounts in your organization
  - entities can access only what is allowed by both the AWS Organizations policies and IAM policies.
- to create and attach a policy to your organization, you must configure that policy type for use.
  - Turning on a policy type is a one-time task on the organization root
- authorization policies
  - service control policies
- management policies
  - AI services opt-out policies
  - backup policies
  - tag policies
- policy inheritance: attach a policy to:
  - the organization root: all OUs and accounts in the organization inherit that policy.
  - a specific OU: accounts that are directly under that OU or any child OU inherit the policy.
  - a specific account: it affects only that account.

##### AI Opt-out Policies

- control data collection for AWS AI services for all your organization's accounts.

##### Backup Policies

- centrally manage and apply backup plans to the AWS resources across your organization's accounts.

##### Tag Policies

- standardize the tags attached to the AWS resources in your organization's accounts.

##### Service Control Policies

- centralize & specify the maximum permissions for OUs and member accounts (including root user)
  - i.e. SCP defines a guardrail, or sets limits, but doesnt grant any access
  - recommendation is to use a "block list" strategy where all actions are implicitly allowed except for those actions you want to block by creating statements that deny access
    - specify resources and conditions for the statement and use the NotAction element.
  - if you ignore the recommendation and use the Allow statement
    - permits the Resource element to only have a "\*" entry.
    - can't have a Condition element at all.
- requires all organization features to be enabled
- FYI
  - do effect
    - users (including root), roles in member accounts of the associated OU
  - don't affect
    - users or roles in the management account.
    - esource-based policies directly
    - any service-linked role.
    - there are some exceptions to services specified in the docs

### STS

- create and provide trusted IAM users/federated users with temporary security credentials that can control access to your AWS resources
- [security token service](./iam-sts.md)

### Identity Center

- manages SSO

### Cognito

- web and mobile apps authN

### access analyzer

- continuously monitors policies for changes

#### for s3

- monitors and evaluates policies & For each public or shared bucket, you receive findings that report the source and level of public or shared access.
  - can download as a CSV report.
- review all buckets that grant public/shared access
- bucket access control lists (ACLs)
- bucket policies,
- access point policies

### Config

- e.g. monitoring unused IAM roles

### lambda

- execution roles
  - AWSLambdaVPCAccessExecutionRole

### api gateway

- IAM policy API permissions
  - execute-api: who can invoke the api/refresh the api cache
  - apigateway: who can manage/create/deploy the api
- resource syntax: `arn:aws:PERM-NAME:REGION:ACCOUNT-ID:API-ID/STAGE-NAME/HTTP-METHOD/API-RESOURCE`

### dynamodb

- authnz at the table, item, or attribute level
- for east/west in the same VPC always use a VPC endpoint for the dynamodb table
  - this prevents traffic from having to treverse the publically routed internet using public addressing

### ecs

- apply roles down to the task level for granular positions
  - assign resourced policies with appropriate permissions to each role
  - Applications are required to sign their AWS API requests with AWS credentials, and this feature provides a strategy to manage credentials for your application’s use
- common roles
  - ecsServiceRole: enables ECS to (de)register container instances from a load balancer whenever tasks are placed on them
- permission tiers
  - fargate
    - cluster permissions: control who can launch/describe tasks in a cluster
    - application permissions: enable containers to access AWS resources
    - task housekeeping permissions: ecr image pull, cloudwatch logs push, eni creation, (de)register targets into elastic load balancing, etc
      - execution role: ECR image pull, pushing cloudwatch logs
      - ecs service linked role: ENI management, ELB target (de)registration

### secrets manager

- apply resource policies directly to individual secrets defining who can access and what they can do
- alternatively apply roles to resources giving them access to specific secrets

### s3

- IAM policies attached to users/groups/roles for authnz of s3 resources

### RDS

- assign permissions to determine who can manage RDS resources
  - e.g. create, describe, modify and delete db instances, tag resources, modify security groups

### eks

- deploying new eks clusters requires at least 3 permissions
  - cluster iam role: eks permissions to make calls to AWS apis on your behalf for cluster management
    - e.g. managing ec2 auto scaling for worker nodes
    - aws provides an IAM policy with the recommended permissions for this role
  - node iam role: the kubelete daemon on eks worker nodes makes calls to aws apis on your behalf
    - e.g. pulling container images from ECR
  - rbac user: humans that manage the k8s cluster need permission to make calls to the k8s api
    - you map an IAM role to a k8s RBAC user
      - create additional principal sin IAM that map to restrict roles in rbac for specific mgmt tasks
    - the role used to create the clsuter will always have sudo access
      - you should instead create a specific role just for deploying clusters

### Ec2

- Amazon EC2 instance profiles provide credentials to EC2 instances.
  - works similarly to IAM roles for ECS tasks

### Step Functions

- a user/role/etc can be given access to AWS Step Functions by directly attaching the existing policies defined in the IAM service
  - AWSStepFunctionsConsoleFullAccess: access policy providing access to the AWS Step Functions console
    - For a full console experience: a user may need iam:PassRole permission on other IAM roles that can be assumed by the service.
  - AWSStepFunctionsFullAccess: access policy providingaccess to the AWS Step Functions API.
  - AWSStepFunctionsReadOnlyAccess: access policy providingread only access to the AWS Step Functions service.
- AWS Step Functions can invoke code and access AWS resources.
  - you need to grant Step Functions access to those resources by using an IAM role.

### Systems Manager

- FYI:
  - for hybrid environments you need a service role that grants the agent access to assumeRole
- required policies
  - resource groups
  - tag editor
  - instance profile role for SSM to manage resources on your behalf
- optional policies
  - personal health dashboard
  - aws Config
  - cloudwatch logs / cloudwatch agent
  - s3 buckets

### neptune

- By default, IAM database authentication is disabled when you create an Amazon Neptune DB cluster
  - Enable IAM DB Authentication -> yes
  - Accessing Neptune with IAM-based authentication requires that you create HTTP requests and sign the requests yourself.
    - check the awscurl tool in the cli file, or use the sdk
- common managed policies
  - NeptuneReadOnlyAccess
  - NeptuneFullAccess
  - NeptuneConsoleFullAccess
