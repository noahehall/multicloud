# Tags

- categorize or organize your AWS resources to ease your management
- a label that you assign to your resources; Each tag consists of a key and an optional value

## links

- [conventions and rules](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html)
- [best practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)
- [resource tagging strategy](https://aws.amazon.com/blogs/mt/implement-aws-resource-tagging-strategy-using-aws-tag-policies-and-service-control-policies-scps/)

## best practices

- tag everything, and managed tag resources; e.g. fine-grained access control, cost allocation, querying/filtering, lifecycle management, analytics, etc
- think about
  - job function/project attributes/cost allocation
  - account/team/organization/etc
  - ABAC/RBAC
- enforcement
  - tag usage: via IAM policies conditions requiring tags
  - resource-level management: via IAM Policies dictating who can create/modify tags
- create a logical, well-architected approach to monitoring and maintaining your resources.

## basics

- tags integrate with everything

## Cost Allocation Tags

- specific to billing and track your AWS costs on a detailed level.
- must be enabled; AWS uses the tags to organize your resource costs on your cost allocation report.
- Cost Allocation Tags appear on the console after you've enabled Cost Explorer, Budgets, AWS Cost and Usage Reports, or legacy reports.
- tag types
  - AWS Generated: automatically applied on a best-effort basis; enable in the Billing and Cost Management console.
    - available only in the Billing and Cost Management console and reports, and doesn't appear anywhere else in the AWS console, including the AWS Tag Editor.
  - User Defined: tags that you define, create, and apply to resources.

## s3 tags

### Bucket tags

- label buckets with AWS-generated tag or user-defined tag
  - generated: AWS defines, creates, and applies the AWS-generated tag, createdBy,
  - user defined: whatever you want

### object tags

- a way to categorize and query your storage
- up to 10 tags per object
- keys can be up to 128 characters in length
- values can be up to 255 characters in length
- Key and tag values are case sensitive

### tag sets

- all of the tags that are assigned to a bucket or object, up to 50
  - keys must be unique
- to modify tag sets via api: download all > modify > upload all

## EBS

- Target tags: are used to identify the resources to back up.

### Data lifecycle Mananger Tags

- DLM resource tags are specific tags applied to all snapshots and AMIs created by a lifecycle policy.
- distinguish them from snapshots and AMIs created by any other means.
- automatically assigns a system-generated tag to each snapshot based on the schedule's frequency.
