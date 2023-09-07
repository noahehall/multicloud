# Organizations

- Centrally manage your environment as you scale your AWS resources
- group accounts into logical categories
- use cases
  - manage access, share resources and consolidate billing/audit at the organization level
  - nest organizations to form hierarchies

## my thoughts

- check control towe for a more managed approach if you can afford it

## links

- [integrations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_integrate_services_list.html)
- [landing page](https://aws.amazon.com/organizations/?did=ap_card&trk=ap_card)
- [managing accounts](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts.html)
- [managing org OUs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_ous.html)
- [managing organization policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies.html)
- [managing orgs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org.html)
- [recommended OUs](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/recommended-ous.html)
- [service control policies: examples](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html)
- [service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
- [service constrol policies: inheritance](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_inheritance_auth.html)

## best practices

- limit access to an organization’s management account.
- limit the cloud resources and data contained in the management account to only those that must be managed in the management account.
- isolate production workload environments and data in production accounts housed within production OUs.
- define one or more non-production OUs that contain accounts and workload environments that are used to develop and test workloads.
- Consider separating workloads that have different owners into their own production accounts to simplify access management, streamline change approval processes, and limit the scope of impact for misconfigurations.
- use AWS identity federation capabilities through either AWS Single Sign-On (AWS SSO) or IAM integration with a third-party identity provider

### anti patterns

## features

- programmatically create new AWS accounts for resources and teams
- simplify user-based authnz for teams and users
- manage and optimize costs across accounts and resources
- centrally secure and audit environments across all acounts
- combine your existing accounts into an organization to manage the accounts centrally.
- Consolidated billing: use the management account of your organization to consolidate and pay for all member accounts
- hierarchical grouping of accounts: group your accounts into organizational units (OUs) and attach different access policies to each OU.
  - nest OUs within other OUs to a depth of five levels to provide flexibility in how you structure your account groups.
- Policies: create and use different types of policies to manage permissions and control actions within individual accounts and groups of account
- extending IAM authnz: giving you control over what users and roles in an account or a group of accounts can do
  - permissions allowed in an account are a combination of those allowed at the account level by AWS Organizations and the IAM permissions explicitly granted at the user or role level.
- Integration with other AWS Services: use the multi-account management services with specific AWS services to perform tasks on all accounts that are members of an organization
  - see link up top

### pricing

## basics

### organization:

- entity that you create to consolidate your AWS accounts so that you can administer them as a single unit
  - To fully manage accounts, turn on all features option for your organization

#### organizational unit: OU;

- Root: the parent or top-level OU for other OUs and account
- Security OU: The Security OU is a foundational OU. Your security organization should own and manage this OU in addition to any child OUs and associated accounts.
- Infrastructure OU: The Infrastructure OU, another foundational OU, is intended to contain shared infrastructure services. Your infrastructure teams should own and manage this OU, any child OUs, and associated accounts.
- Sandbox OU: The Sandbox OU contains accounts that your developers can generally use to explore and experiment with AWS services and other tools and services, subject to your acceptable use policies. These environments are typically disconnected from your internal networks and internal services.
- Workloads OU: The Workloads OU is intended to house most of your business-specific workloads, including both production and non-production environments. These workloads can be a mix of commercial off-the-shelf (COTS) applications and your own internally developed custom applications and data services. Workloads in this OU often include shared application and data services that are used by other workloads.
- Policy Staging OU: The Policy Staging OU is intended to help teams that manage overall policies for your AWS environment to safely test potentially broadly impacting policy changes before applying them to the intended OUs, accounts, or both.
- Suspended OU: The Suspended OU is used as a temporary holding area for accounts that are required to have their use suspended either temporarily or permanently.
- Individual Business Users OU: The Individual Business Users OU houses accounts for individual business users and teams who need access to directly manage AWS resources outside the context of resources managed within your Workloads OU.
- Exceptions OU: The Exceptions OU houses accounts that require an exception to the security policies that are applied to your Workloads OU. Normally, there should be a minimal number of accounts, if any, in this OU.
- Deployments OU: The Deployments OU contains resources and workloads that support how you build, validate, promote, and release changes to your workloads.
- Transitional OU: The Transitional OU is intended as a temporary holding area. It is used for existing accounts and workloads that you move to your organization before you formally integrate them into your more standardized areas of your AWS environment structure.

##### Account:

- A standard AWS account that contains your AWS resources and the identities that can access those resources

###### management account

- the payer account and is responsible for paying all charges that are accrued by the member accounts.
- used to create the organization and has the highest level of management for the organization.
- create/remove/invite other accounts
- Apply policies to entities (roots, OUs, or accounts) within the organization.
  - at the root level
  - at the OU level
  - at the account level
- Configure integration with supported AWS services to provide service functionality across all accounts in the organization.

###### member accounts:

- make up the rest of the accounts in an organization.
- An account can be a member of only one organization at a time.

### organization-based strategy

- separating your cloud-infrastructure to protect and limit access to resources at different levels.
- separating resources and centralized management
  - Restrict or limit access to your AWS resources using the principle of least privilege.
  - separate:
    - accounts for different applications and workflows.
    - active data from backup and archive data.
  - centrally:
    - manage and enforce access privileges.
    - enforce development and deployment standards.
    - monitor resources and activities.
- key benefits of a multi-account strategy
  - group workloads by business purpose and ownership
  - apply distinct security controls by env
  - constrain access to sensitive data
  - limit the scope of impact from adverse events
  - manage costs

### Security

#### Policies

- apply additional types of management to the AWS accounts in your organization
- to create and attach a policy to your organization, you must configure that policy type for use.
  - Turning on a policy type is a one-time task on the organization root
- authorization policies
  - service control policies
- management policies
  - AI services opt-out policies
  - backup policies
  - tag policies
- policy inheritance: attach a policy to:
  - the organization roo: all OUs and accounts in the organization inherit that policy.
  - a specific OU: accounts that are directly under that OU or any child OU inherit the policy.
  - a specific account: it affects only that account.

##### AI Opt-out Policies

- control data collection for AWS AI services for all your organization's accounts.

##### Backup Policies

- centrally manage and apply backup plans to the AWS resources across your organization's accounts.

##### Tag Policies

- standardize the tags attached to the AWS resources in your organization's accounts.

##### Service Control Policies

- see the IAM file
- specify the maximum permissions for member accounts in the organization
- centralize control over the AWS services and API actions that each account can access

## considerations

## integrations

- theres an `integrations` link up top

### s3

- activate S3 Storage Lens trusted access using your AWS Organizations management
- collects metrics and usage data for all AWS accounts that are part of your AWS Organization's hierarchy.
