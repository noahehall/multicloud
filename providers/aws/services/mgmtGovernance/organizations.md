# Organizations

- Centrally manage your environment as you scale your AWS resources
- group accounts into logical categories
- use cases
  - manage access, share resources and consolidate billing/audit at the organization level
  - nest organizations to form hierarchies

## my thoughts

- check control towe for a more managed approach if you can afford it

## links

- [landing page](https://aws.amazon.com/organizations/?did=ap_card&trk=ap_card)
- [service control policies: examples](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_example-scps.html)
- [service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
- [organization policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies.html)
- [integrations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_integrate_services_list.html)

## best practices

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

- organization: entity that you create to consolidate your AWS accounts so that you can administer them as a single unit
- Root: The parent container for all the accounts for your organization
- organizational unit: OU; A container for accounts within a root and for other OUs,
-

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

#### Authnz

##### Service Control Policies

- see the IAM file
- specify the maximum permissions for member accounts in the organization

## considerations

## integrations

- theres an `integrations` link up top

### s3

- activate S3 Storage Lens trusted access using your AWS Organizations management
- collects metrics and usage data for all AWS accounts that are part of your AWS Organization's hierarchy.
