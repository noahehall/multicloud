# Cost and Usage Reports (CUR)

- provides the most comprehensive source of information
- Dive deeper into your AWS cost and usage data

## my thoughts

## links

- [landing page](https://aws.amazon.com/aws-cost-management/aws-cost-and-usage-reporting/?did=ap_card&trk=ap_card)
- [CUR: intro](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html)

## best practices

### anti patterns

## features

- Understand cost drivers at the resource level and identify cost optimization opportunities.
- Organize cost and usage data with your own AWS cost categories and cost allocation tags.
- Create and publish billing reports to break down your cloud costs.
- Track your savings plan use: Amortize associated fees and calculate internal cost allocations based on your organization’s reporting needs.
- Query your cost and usage information with data integration, or perform deeper data analyses using other AWS services

### pricing

## basics

### Cost and Usage Report (CUR)

- download the reports from the S3 console, query the report using Amazon Athena, or upload the report into Amazon Redshift or Amazon QuickSight.

### Billing reports

- separates your usage information and cost by AWS service and function
- types
  - monthly report
  - cost allocation report
  - detailed billing report

### Cost Allocation Tags

- specific to billing and track your AWS costs on a detailed level.
- must be enabled; AWS uses the tags to organize your resource costs on your cost allocation report.
- Cost Allocation Tags appear on the console after you've enabled Cost Explorer, Budgets, AWS Cost and Usage Reports, or legacy reports.
- tag types
  - AWS Generated: automatically applied on a best-effort basis; enable in the Billing and Cost Management console.
    - available only in the Billing and Cost Management console and reports, and doesn't appear anywhere else in the AWS console, including the AWS Tag Editor.
  - User Defined: tags that you define, create, and apply to resources.

## considerations

## integrations

### Tags

### Athena

### QuickSight

### RedShift
