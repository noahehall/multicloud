# cloudtrail

- captures all AWS service API calls as events, including calls from the console, AWS CLI, and API tools

## my thoughts

- become one with cloudtrail

## links

- [concepts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html)
- [dynamodb: log file entries](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/understanding-ddb-log-entries.html)
- [dynamodb](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html)
- [event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)
- [log files](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-working-with-log-files.html)
- [logging data events](https://docs.amazonaws.cn/en_us/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)
- [neptune](https://docs.aws.amazon.com/neptune/latest/userguide/cloudtrail.html)
- [sending trail events to cloudwatch logs](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/send-cloudtrail-events-to-cloudwatch-logs.html)
- [user guide intro](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [vpc: transit gateway](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-cloudtrail-logs.html)
- [event field reference](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)

## best practices

- careful with custom trails: some are high volume (e.g. lambda invocations) and can incur major costs
- multi region configuration: deliver log files from multiple regions to a single Amazon S3 bucket for a single account

### anti patterns

## features

- create a trail for captured events and store in s3/cloudatch log/events
- enables governance, compliance, operational auditing and risk auditing of an aws account
- log, monitor and retain account activity related to actions across aws infrastructure
- event history of actions taking through the management console / sdks / clis / etc
- simplify security analysis, resource change tracking and troubleshooting

### pricing

## terms

## basics

- you create a trail that delivers log files to an s3 bucket/cloudwatch logs
- since all actions against your account can be recorded: the primary goal is the effectively analysis of and reaction to those trails
  - who made the request
  - when and from where
  - what happened

### events

- provides a viewable, searchable, and downloadable record of the past 90 days of CloudTrail events

#### Data Plane Events

- record object-level API activity and receive detailed information such as who made the request, where and when the request was made, and other details
- record the resource operations (data plane actions) performed on or within the resource itself.
- often high volume activities.
- not logged by default when you create a trail.

#### control plane events

- management events provide insights into the management (control plane) operations performed on resources in your AWS account

### trails

- configuration that enables delivery of CloudTrail events as log files to an s3 bucket
- option to
  - automatically monitor and alarm on specified events by sending log events to Amazon CloudWatch Logs
  - query logs and analyze AWS service activity with Amazon Athena
- automatically keeps 90-day history of events; create your own trail to maintain a longer history.

#### insights

- configure CloudTrail Insights on your trails to identify and respond to unusual activity associated with write API calls
  - tracks your normal patterns of API call volume and generates Insights events when the volume is outside normal patterns.

#### logs

- log files are written in JSON format
- Each unique event is a single JSON object

##### important log fields

- eventTime: when the request was made in UTC
- eventType: referenced by the event
  - AwsConsolSignin: A user in your account (root, IAM, federated, SAML, or SwitchRole) signed in to the AWS Management Console.
  - AwsServiceEvent: The called service generated an event related to your trail
  - AwsApiCall: A public API for an AWS resource was called
- eventSource: The service that the request was made to; generally the service endpoint
  - e.g. CloudWatch is monitoring.amazonaws.com
- eventName: The requested action, which is one of the actions in the API for that service.
- sourceIPAddress: IP address or DNS name of the calling service is used
- userAgent: The tool or application through which the request was made
  - signin.amazonaws.com – The request was made by an IAM user with the AWS Management Console.
  - console.amazonaws.com – The request was made by a root user with the AWS Management Console.
- errorMessage: Any error message returned by the requested service.
- requestParameters: The parameters that were sent with the API call
  - provide additional details about the event
- resources: List of AWS resources accessed in the event. This can be the resource's ARN, an AWS account number, or the resource type.
- userIdentity: A collection of fields that describe the user or service that made the call.
  - Root: If the userIdentity type is Root and you set an alias for your account, the userName field contains your account alias.
  - IAMUser
  - AssumedRole: The request was made with temporary security credentials that were obtained with a role via a call to the AWS STS AssumeRole API call.
  - FederatedUser: The request was made with temporary security credentials that were obtained via a call to the AWS STS GetFederationToken API
  - AWSAccount:
  - AWSService: The request was made by an AWS service.
- responseElements: provide context to the request for actions that make changes
  - if an action doesnt make changes, these field is omitted
- managementEvent: true if its a control plane event

### security

### Encryption

- check the kms file

## considerations

## integrations

- cloudtrail is a fundamental service and integrates with everything

### IAM

- particularly useful in seeing why an api called failed
  - which service, action and policy was used

### cloudwatch

- its all about setting alarms when changes are made against production resources

### athena

- for running deeper analysis on trails saved to s3

### lambda

- default logging is for control plane (mgmt) events
- optional logging: can turn on data event logging for tracking every time lambda fns are invoked

### api gateway

### Key Management Service (KMS)

- usage of managed keys are recorded in cloudtrail
- inspect whos making the request, services used, actions performed, parameters for the action and response elements returned

### s3

- can track object level data events, e.g. uploads to a bucket

### dynamodb

### ebs

- common events
  - control plane
    - {Start,Complete}Snapshot
  - data plane:
    - {List,Get,Put}SnapshotBlocks, ListChangedBlocks
