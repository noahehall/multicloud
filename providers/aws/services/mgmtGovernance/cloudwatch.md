# cloudwatch

- centralized solution to monitor resources and applications on AWS, on premise and other clouds
- Collect and track metrics, monitor log files, and set alarms and define automation

## links

- [agent network performance metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-network-performance.html)
- [agent: configuration](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html)
- [agent: install](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)
- [agent: metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/metrics-collected-by-CloudWatch-agent.html)
- [agent: troubleshooting](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/troubleshooting-CloudWatch-Agent.html)
- [alarms: anomaly detetion](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create_Anomaly_Detection_Alarm.html)
- [alarms: static thresholds](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html)
- [api gateway: metrics](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-metrics-and-dimensions.html)
- [billing: alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)
- [container insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights.html)
- [cross account cloudwatch ](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Cross-Account-Cross-Region.html)
- [dashboards: alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/add_remove_alarm_dashboard.html)
- [dashboards: animation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-animated-dashboard.html)
- [dashboards: metrics explorere](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/add_metrics_explorer_dashboard.html)
- [dashboards: sharing](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-dashboard-sharing.html)
- [embedded metric format: intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format.html)
- [embedded metric format: specification](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format_Specification.html)
- [getting started](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)
- [log insights](https://aws.amazon.com/blogs/aws/new-amazon-cloudwatch-logs-insights-fast-interactive-log-analytics/)
- [logging intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [logs export to s3](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/S3Export.html)
- [logs: intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [metrics : explorer](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Metrics-Explorer.html)
- [metrics: by service](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html)
- [metrics: intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
- [metrics: using metric math](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html)

### api

- [AAA all actions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_Operations.html)
- [AAA api landing page](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/Welcome.html)
- [getMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)
- [start query](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_StartQuery.html)

### tools

- [cloudwatch with grafana](https://grafana.com/docs/grafana/latest/datasources/aws-cloudwatch/)

## my thoughts

- everything starts and ends with cloudwatch
- cloudwatch is the gateway integration to SNS, SQS and lambda (etc) for automated response and remediation patterns

## best practices

- structure application-level logs following the EMF standard
  - instruct CloudWatch Logs to automatically extract metric values that are embedded in structured log events.
- be careful about enabling logging of full requests in production and leaking sensitive data
- alarms
  - the time period should be for a sustaind amount of time, and not short bursts/temporary spikes in metrics
  - recommended alarms
    - create, update, or delete a:
      - VPC customer or internet gateway.
      - network ACL.
      - security group.
      - CloudTrail trail or when the logging process defined by a trail is stopped or started.
    - every time an AWS root account is used.
    - alarm is created and configured for the metric filter attached to the VPC Flow Logs CloudWatch log group to receive notifications when IP packets are rejected inside the specified VPC
      - Ensure that a metric filter that matches the pattern of the rejected traffic is created for the CloudWatch log group assigned to VPC Flow Logs.
- reducing cloudwatch costs
  - Only use detailed monitoring when needed.
  - Remove any unnecessary alarms.
  - Disable monitoring of custom metrics.
  - Delete unnecessary dashboards.
  - Stop ingesting logs that are not needed.
  - Run queries for shorter durations.

### anti patterns

## features

- collect, assess and analyze resource and application data using visualization tools
- improve operational performance using alarms and automated actions at predetermined thresholds
- integrate with 70+ aws services for a centralized solution
- troubleshoot operational problems with actionable insights derived from logs and metrics
- monitor application performance, perform root cause analysis, optimize resources

### pricing

- the free tier
  - basic monitoring: visibility into aws resources without any extra costs
    - resources automatically send metrics to cloudwatch for free at a rate of 1 data point per metric per 5-minute interval
- the paid tier: your best bet is to use the aws calculator; depends on the region and is split out by feature
  - logs
  - metrics
  - dashboards
  - alarms
  - events
  - contributor insights
  - canaries
  - evidently
  - rum
  - metrics insights
  - cross-account observability
  - internet monitor
- a good heuristic
  - CloudWatch alarm with multiple metrics, you are charged for each metric associated with a CloudWatch alarm.
  - exceed three dashboards with up to 50 metrics.
  - ingesting and storing logs, as well as the amount of ingested logs scanned for each CloudWatch Insights query.
  - number of custom events.
  - monitor more than 10 custom metrics
    - Metrics collected by the CloudWatch agent are billed as custom metrics.

## basics

- general workflow
  - most services automatically/require minimumal configuration to publish to cloudwatch
    - for application-level metrics, you'll need to do this yourself
  - once log data is in cloudwatch
    - setup a metric filter
    - create alarms
    - create actions

### dashboards

- customizeable home pages configured for one/more metrics through widgets pulled from one/more regions

#### Cross Account Cloudwatch

- gives a central AWS account the ability to receive CloudWatch metrics from other AWS accounts
- monitor several accounts from a single dashboard

### metrics

- metrics become events become alarms
- REGION SPECIFIC time-ordered set of data points
  - cannot be deleted, but automatically expire after 15 months if no new data is published to them.
  - Each data point must be associated with a time stamp.
    - up to two weeks in the past and up to two hours into the future
    - by default its time the data point was received.
  - to aggregate statistics from different Regions, use cross-Region functionality
- custom metrics enable you to post application-level metrics to cloudwatch
- you can run `math metrics` across metrics e.g. errors/invocations
- namespaces
  - Each namespace contains data and each different namespace holds different data
    - service namespaces: AWS/{NetworkManager,TransitGateway,InsertServiceNameHere}
    - CloudWatch agent: CWAgent
  - create custom namespaces to collect data on your applications and system
    - `AWS/Blah` is reserved for AWS
- retention:
  - the longer CloudWatch data is stored, the less information is available, due to aggregation of those data points.
  - Data points with a period of
    - less than 1 minute are available for 3 hours.
      - These data points are high-resolution custom metrics.
    - 1 minute are available for 15 days
    - 5 minutes are available for 63 days
    - 1 hours are available for 455 days (15 months)

#### dimensions

- a name/value pair that is part of the identity of a metric
  - assign up to 10 dimensions to a metric.
- separate your data points for different measurements

#### Metrics Explorer

- monitor resources by their tags and properties.
- tag-based tool filters, aggregates, and visualizes metrics, adding enhanced observability

### logs

- centralized place for logs to be stored, queried and analyzed
- execution logging: what occurs during a service action; useful for troubleshooting services
- access logging: whos invoking a service action; fully customizable

#### Log Events

- a record of some activity recorded by the application or resource being monitored.
- contains two properties:
  - the timestamp of when the event occurred
  - the raw event message. Event messages must be UTF-8 encoded.

#### Log Groups

- define groups of log streams that share the same retention, monitoring, and access control settings.
- can also export log data from your log groups to an Amazon Simple Storage Service (S3) bucket and use this data in custom processing and analysis, or to load onto other systems

##### Log Streams

- a sequence of log events that share the same source
- generally intended to represent the sequence of events coming from the application instance or resource being monitored.
- Each log stream has to belong to one log group

##### Retention Settings

- specify how long log events are kept in CloudWatch Logs.
- expired log events get deleted automatically.
- are also assigned to log groups, and the retention assigned to a log group is applied to its log streams.

##### metric filters

- allow you to sort groups of related objects within a single bucke
  - filter by object key name prefix or by the object tag.
- how cloudwatch turns log data into numerical cloudwatch metrics that you can graph and use on dashboards or set an alarm on.
- are assigned to log groups, all of the filters assigned to a log group are applied to their log streams.

#### log agent

- runs on EC2 to automatically send log data to cloudwatch logs

### alarms

- automatically initiate actions based on sustained state changes of metrics
  - Create up to 5,000 alarms per Region per AWS account.
  - Only use ASCII characters for alarm names.
- invoked when it transitions from one state to another
  - alarms with Auto Scaling actions: continues to invoke the action once per minute that the alarm remains in the new state.
- alarm evaluation
  - period: seconds to evaluate the metric or expression to create each individual data point for an alarm
  - evaluation periods: number of the most recent periods, or data points, to evaluate when determining alarm state.
  - datapoints to alarm: number of data points within the evaluation periods that must be breaching to cause the alarm to go to the ALARM state
    - The breaching data points don't have to be consecutive;
    - just must all be within the last number of data points equal to the Evaluation Period.
- creating alarms
  - choose the metric to monitor
  - choose the metric alarm state
  - configure alarm
    - period
    - evaluation periods
    - data points to alarm
  - choose when to initiate and what actions to take

#### Metric Alarm

- watches a single CloudWatch metric or the result of a math expression based on CloudWatch metrics
- performs one or more actions based on the value of the metric or expression relative to a threshold over a number of time periods.
- alarm states
  - ok: the metric is within the defined threshold
  - alarm: the metric is outside the defined threshold
  - insufficient_data: alarm has just started, metric is not available, or not data to determine alarm start
- each specific data point reported to CloudWatch is also in one of three states:
  - Not breaching (within the threshold)
  - Breaching (violating the threshold)
  - Missing
- specify CloudWatch to treat missing data points as:
  - notBreaching: within threshold (good)
  - breaching: outside threshold (cont towards trigger)
  - ignore: current alarm state is maintained
  - missing: If all data points in the alarm evaluation range are missing, the alarm transitions to INSUFFICIENT_DATA.

#### Composite Alarm

- includes a rule expression that takes into account the alarm states of other alarms that you have created.
- goes into an ALARM state only if all conditions of the rule are met.
  - Using composite alarms can reduce alarm noise.

#### actions

- theres to many to list here, check the link up top
- getMetricData: retrieve cloudwatch metric values

### events

- see the eventbridge file: the new tool used to field and respond to events

#### Event Rules

- A rule matches incoming events and sends them to targets for processing
- A single rule can send an event to multiple targets, which then run in parallel.
- based either on an event pattern or a schedule
  - pattern: defines the event structure and the fields that a rule matches
  - schedule: perform an action at regular intervals.

#### Event Buses

- abcd

### Agent

- Some CloudWatch metrics are not/cant be collected by default.
  - standard metrics are collected through use of the hypervisor, it has no way to gather information related to the instance's operating system and applications
  - thats why you need the Agent installed
- helps you monitor your EC2 instances and volumes by collecting in-guest system metrics
  - collect system-level metrics including disk and memory
  - collect data at a sub-resource level, such as per-disk and disk-specific metrics
- imports network performance metrics for EC2 instances running on Linux using the Elastic Network Adapter (ENA) to publish network performance metrics to CloudWatch.

#### application monitoring

### configuration

- a JSON file that specifies the metrics and logs that the agent is to collect, including custom metrics
- use cli Wizard to get the latest default configuration
  - Any time you change the agent configuration file, you must then restart the agent for the changes to take effect.

```sh

# Start the CloudWatch agent configuration wizard by entering the following command:
## theres a windows version --- fk windows
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
## config file location: /opt/aws/amazon-cloudwatch-agent/bin/config.json
```

```jsonc
// sample configuration
{
  "agent": {
    "metrics_collection_interval": 60,
    "region": "us-west-1",
    "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
    "debug": false,
    "run_as_user": "cwagent"
  }
}
```

### insights

#### Query language

- use to perform queries on your log groups.
- One or more query commands separated by Unix-style pipe characters (|).
- Six query commands, along with many supporting functions and operations
- scheduling a query: a query timesout after 5 minutes
  - Log group
  - time range to query
  - query string to use

```sh
# example queries

## top 15 packet transfers across hosts
stats sum(packets) as packetsTransferred by srcAddr, dstAddr
  | sort packetsTransferred  desc
  | limit 15

## top 15 byte transfers for hosts in a subnet
filter isIpv4InSubnet(srcAddr, "192.X.X.X/24")
  | stats sum(bytes) as bytesTransferred by dstAddr
  | sort bytesTransferred desc
  | limit 15

## protocol numbers: https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml
## ip addrs that use a specific protocol
filter protocol=6 | stats count(*) by srcAddr

## 25 most recent log events
fields @timestamp, @message | sort @timestamp desc | limit 25

## list of log events that are not exceptions
fields @message | filter @message not like /Exception/
```

#### container insights

- creates automatic dashboards, and are also viewable in the cloudwatch console

#### lambda insights

- a special monitoring and troubleshooting solution for serverless applications running on Lambda
- uses a CloudWatch Lambda extension, which is provided directly at the Lambda layer. When you install this extension on your Lambda function, it collects system-level metrics and emits a single performance log event for every invocation of that Lambda function.

#### contributor insights

- a diagnostic tool

#### application insights

#### log insights

- interactively search and query and analyze log data in Cloudwatch Logs
- will automatically extract fields from structured logs
- use prebuilt or custom queries on your logs to provide aggregated views and reporting
- 1,000 CloudWatch Logs Insights queries, per Region per account.
- default system fields
  - @message: raw unparsed log event
  - @timestamp: in the log event's timestamp field. This is equivalent to the timestamp field in InputLogevent.
  - @ingestionTime: time when the log event was received by CloudWatch Logs.
  - @logStream: name of the log stream that the log event was added to.
  - @log: identifier in the form of account-id:log-group-name;
    - useful in queries of multiple log groups, to identify which log group a particular event belongs to.

## considerations

## integration

- [check the markdown file](./cloudwatch-integrations.md)
