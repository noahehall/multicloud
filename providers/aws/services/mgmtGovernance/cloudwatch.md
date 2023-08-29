# cloudwatch

- centralized solution to monitor resources and applications on AWS, on premise and other clouds
  - Collect and track metrics, collect and monitor log files, and set alarms.
- CloudWatch Metrics: monitoring & billing, observability
- CloudWatch Logs: aggregator

## links

- [api gateway: metrics](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-metrics-and-dimensions.html)
- [by service](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html)
- [container insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights.html)
- [dynamodb: alarms](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/creating-alarms.html)
- [dynamodb: contributor insights](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/contributorinsights_HowItWorks.html)
- [dynamodb: contributor insights tut](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/contributorinsights_tutorial.html)
- [dynamodb: metrics & dimensions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/metrics-dimensions.html)
- [dynamodb: monitoring](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/monitoring-cloudwatch.html)
- [ecs: monitoring](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_monitoring.html)
- [eks: pushing logs to container insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-EKS-logs.html)
- [embedded metric format: intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format.html)
- [embedded metric format: specification](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format_Specification.html)
- [fsx: metrics](https://docs.aws.amazon.com/fsx/latest/LustreGuide/monitoring_overview.html)
- [getting started](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)
- [kinesis: metrics](https://docs.aws.amazon.com/streams/latest/dev/monitoring-with-cloudwatch.html)
- [lambda: analyzing behavior](https://aws.amazon.com/blogs/mt/understanding-aws-lambda-behavior-using-amazon-cloudwatch-logs-insights/)
- [lambda: metrics](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-metrics.html)
- [log insights](https://aws.amazon.com/blogs/aws/new-amazon-cloudwatch-logs-insights-fast-interactive-log-analytics/)
- [logging intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [logs export to s3](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/S3Export.html)
- [logs: intro](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [sns: metrics](https://docs.aws.amazon.com/sns/latest/dg/sns-monitoring-using-cloudwatch.html)
- [sqs: metrics](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html)
- [step functions: metrics](https://docs.aws.amazon.com/step-functions/latest/dg/procedure-cw-metrics.html)
- [using metric math](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html)
- [using metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
- [agent network performance metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-network-performance.html)

### api

- [AAA all actions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_Operations.html)
- [AAA api landing page](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/Welcome.html)
- [getMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)

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

### anti patterns

## features

- collect, assess and analyze resource and application data using visualization tools
- improve operational performance using alarms and automated actions at predetermined thresholds
- integrate with 70+\_ aws services for a centralized solution
- troubleshoot operational problems with actionable insights derived from logs and metrics
- monitor application performance, perform root cause analysis, optimize resources

### pricing

- the free tier is pretty good
  - basic monitoring: visibility into aws resources without any extra costs
    - resources automatically send metrics to cloudwatch for free at a rate of 1 data point per metric per 5-minute interval
- the paid tier: your best bet is to use the aws calculator
  - depends on the region and is split out by feature
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

## basics

- general workflow
  - most resources automatically/require minimumal configuration to publish to cloudwatch
    - for application-level metrics, you'll need to do this yourself
  - once log data is in cloudwatch
    - setup a metric filter
    - define an alarm
    - define an action

### dashboards

- customizeable home pages configured for one/more metrics through widgets pulled from one/more regions

### metrics

- custom metrics enable you to post application-level metrics to cloudwatch
- you can run `math metrics` across metrics, e.g. errors/invocations

### logs

- centralized place for logs to be stored, queried and analyzed
- execution logging: what occurs during a service action; useful for troubleshooting services
- access logging: whos invoking a service action; fully customizable
- log event: record of activity consisting of a timestamp and an event message
- log stream: sequences of log events that all belong to the same resource
- log group: composed of log streams that all share the same retention and permissions settings

#### metric filters

- how cloudwatch turns log data into numerical cloudwatch metrics that you can graph and use on dashboards

#### log agent

- runs on EC2 to automatically send log data to cloudwatch logs

#### log insights

- interactively search and query and analyze log data in Cloudwatch Logs
- will automatically extract fields from structured logs
- use prebuilt or custom queries on your logs to provide aggregated views and reporting

### alarms

- automatically initiate actions based on sustained state changes of metrics
- invoked when it transitions from one state to another
  - ok: the metric is within the defined threshold
  - alarm: the metric is outside the defined threshold
  - insufficient_data: alarm has just started, metric is not available, or not data to determine alarm start
- you configure when alarms are invoked and the action that is peformed
  - metric: to be monitored
  - threshold: when events breach this number, cloudwatch starts the countdown
  - time period: once the metric exceeds the threshold for this duration, the alarm is triggered

#### actions

- abcd
- getMetricData: retrieve cloudwatch metric values

### events

- when metrics are logged to cloudwatch, an Event can be configured to deliver near real-time streams that describe changes in aws resources

#### rules

- match events and route them to one/more target functions or streams

### application monitoring

### insights

#### container insights

- creates automatic dashboards, and are also viewable in the cloudwatch console

#### lambda insights

- a special monitoring and troubleshooting solution for serverless applications running on Lambda
- uses a CloudWatch Lambda extension, which is provided directly at the Lambda layer. When you install this extension on your Lambda function, it collects system-level metrics and emits a single performance log event for every invocation of that Lambda function.

#### contributor insights

- a diagnostic tool

#### application insights

### failure management

- its all about monitoring and alarms
- ensure your services are persisting logs to cloudwatch logs
  - managed services: stdout is automatically posted to cloudwatch
- exponential backoff and retry logic needs to be included in your application logic
  - utilize the aws sdk for defaults

## considerations

## integration

- [check the markdown file](./cloudwatch-integrations.md)
