# Cloudwatch Integrations, Metrics and Events

- catchall for
  - integratin patterns
  - common metrics and events for various AWS services

## links

- [dynamodb: alarms](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/creating-alarms.html)
- [dynamodb: contributor insights tut](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/contributorinsights_tutorial.html)
- [dynamodb: contributor insights](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/contributorinsights_HowItWorks.html)
- [dynamodb: metrics & dimensions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/metrics-dimensions.html)
- [dynamodb: monitoring](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/monitoring-cloudwatch.html)
- [ec2: automation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/UsingAlarmActions.html)
- [ec2: cpu usage alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_AlarmAtThresholdEC2.html)
- [ec2: enhanced networking](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking.html)
- [ecs: monitoring](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_monitoring.html)
- [eks: pushing logs to container insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-EKS-logs.html)
- [elb: load balancing alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_AlarmAtThresholdELB.html)
- [fsx: metrics](https://docs.aws.amazon.com/fsx/latest/LustreGuide/monitoring_overview.html)
- [kinesis: metrics](https://docs.aws.amazon.com/streams/latest/dev/monitoring-with-cloudwatch.html)
- [lambda: analyzing behavior](https://aws.amazon.com/blogs/mt/understanding-aws-lambda-behavior-using-amazon-cloudwatch-logs-insights/)
- [lambda: metrics](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-metrics.html)
- [neptune](https://docs.aws.amazon.com/neptune/latest/userguide/cloudwatch.html)
- [neptune: cloudwatch logs](https://docs.aws.amazon.com/neptune/latest/userguide/cloudwatch-logs.html)
- [sns: metrics](https://docs.aws.amazon.com/sns/latest/dg/sns-monitoring-using-cloudwatch.html)
- [sns: setup](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html)
- [sqs: metrics](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html)
- [step functions: metrics](https://docs.aws.amazon.com/step-functions/latest/dg/procedure-cw-metrics.html)
- [systemManager: alarm create opsItems](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter-create-OpsItems-from-CloudWatch-Alarms.html)
- [vpc: traffic mirror metrics](https://docs.aws.amazon.com/vpc/latest/mirroring/traffic-mirror-cloudwatch.html)
- [vpc: transit gateway metrics](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-cloudwatch-metrics.html)
- [vpc: transit gateway monitoring](https://docs.aws.amazon.com/vpc/latest/tgw/monitoring-cloudwatch-metrics.html)

## basics

- all AWS services automatically report some information to cloudwatch
  - The metrics in the CloudWatch console are raw and provide more statistics options than the metrics in the service console
- there are 3rd party tools to help analyze this information
- common goals across all integrations
  - operational issues: overutilization, application flaws, misconfiguration or security-related events
  - cost optimization: underutilization
  - scaling, alarms, status checks
  - resource optimization: network utilization, response times, traffic i/o, storage/diskspace consumption/data throughput

## SNS

- cloudwatch alarms that send SNS messages is a foundational pattern in AWS

## vpc

- Replication traffic are NetworkIn, InboundPackets
- Route table changes are ReplaceRoute, ReplaceRouteTableAssociation
- NetworkMirrorIn (traffic mirror)
- namespace for VPC Peering connections AWS/PCX

## lambda

- lambda sends metrics about invocation to cloudwatch
  - including anything sent to stdout
- build graphs and dashboards, set alarams to respond to changes in use, perf or error rates
- metrics
  - invocations: total function executions that were not throttled/failed to start, success & errors
    - errors: invocations that result in a fn error whether by your fn handler or the lambda runtime (timeouts, configuration issues, etc)
    - throttled: failed invocations becaue of concurrency limits; not counted in invocation totals; lambda rejected executed requests because no concurrency was available
  - duration: total time spent processing events; to calculate billing round up to the nearest millisecond
    - iteratorAge: age of the last record in the event releative to event source mappings that read from streams;
      - the agent is the total duration from when the stream receives the record until the event source mapping sends the event to the fn
  - DeadLetterErrors: for async invocation; number of times lambda attempts to send an event to a dead-letter-queue but fails
  - ConcurrentExecutions: fns with custom cuncurrency limits: total concurrent invocations for a given fn at a given point in time
  - UnreservedConcurrentExecutions: fns without a custom cuncurrency limit: total concurrent invocations for a given fn at a given point in time
  - ProvisionedConcurrentExecutions: number of fn isntances that are processing events on provisioned concurrency

## dynamodb

- common metrics/alarms
  - SuccessfulRequestLatency, Throttling Events, Capacity Consumption, System Errors
  - Read usage, Write usage, and Read/Write throttled requests
  - table: ConsumedReadCapacityUnits, ConsumedWriteCapacityUnits, Provisioned{Read,Write}CapacityUnits
  - table ops: ReturnedItemCount, SuccessfulRequestLatency
  - account: AccountMax{Reads,Writes},AccountMaxTableLevel{Reads,Writes}, AccountProvisioned{Read,Write}CapacityUtilization, MaxProvisionedTable{Read,Write}CapacityUtilization, UserErrors
  - throttling: {OnlineIndex,Read,Write}ThrottleEvents, Throttled{PutRecordCount,Requests}
- When you turn on auto scaling for a table, DynamoDB automatically creates several alarms that can launch auto scaling actions.
- Contributor Insights for DynamoDB: identifying the most frequently accessed and throttled keys in your table or index at a glance.
  - rule formats: DynamoDBContributorInsights-XXXXXX-[resource_name]-[creationtimestamp]
    - in your table or global secondary index
      - partition keys of the most
        - PKC: accessed items
        - PKT: throttled items
      - partition and sort keys of the most
        - SKC: accessed items
        - SKT: throttled items

## api gateway

- turn on detailed metrics in stage settings fora extract cost
- monitor and log deployed apis, enabled per stage
- common questions
  - how often apis are called
  - number of api invocations
  - latency of api responses
  - 4 vs 5xx errors
  - cache to hit:miss ratio
- common metrics/alarms
  - CacheHitCount: for rest api cache in given period
  - CacheMissCount: for rest api cache in a given period
  - count: total api requests in a period
  - latency: full roundtrip time between api gateway receiving a request and when it returns a response; includes IntegrationLatency + gateway overhead
  - integrationLatency: time between gateway invoking a target and receiving a response
    - latency - integrationLatency = api gateway overhead
  - 4xxError: total client-side errors _captured_ in a specific period
  - 5xxError: total server-side errors _captured_ in a specific period

## ecs

- cloudwatch logs: make sure specify `awslogs` as the logConfiguration.driver
- cloudwatchEvents: captures task state changes
- service cpu/memory utilization metrics: enables you to scale in/out

### MQ

- broker utilization, queue and topic metrics
- alarms and autoscaling based on metrics

## ec2

- common metrics
  - CPUUtilization
  - NetworkinIn
  - NetworkOut
- detailed monitoring: apps running on EC2 can post metrics every minute (instead of every 5 with basic monitoring)

### enhanced networking:

- uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types
  - requires the cloudwatch agent to be installed

## RDS

- common metrics
  - DatabaseConnections

## ACM

- automate and response to ACM events, e.g. change in certificate state or renewal

## EKS

- container insights can be configured to collect, aggregate and visualize metrics and logs
- also provides diagnostic information, e.g. container restart failures
- general workflow
  - a log collector with a cloudwatch plugin (e.g. fluentd/fluentbit) runs as a DaemonSet on every node
  - the cloudwatch agent runs as a daemonset on each worker node
    - collects and ships metrics data to cloudwatch for processing
  - collect EKS control plane metrics by turning on control plane logging for an EKS cluster
    - cloudwatch collects metrics info from this logs that can be viewed in Log Insights
  - metrics are viewable in Container Insights and Log Insights
- common metrics
  - standard: CPU, memory, disk and network
  - diagnostic metrics: container restart failures

## kinesis

- common metrics
  - iteratorAge: when it increases (lambda consumer) likely means you need to reshard

## EBS

- common metrics
  - bandwidth, throughput, latency, VolumeReadBytes, VolumeWriteBytes, VolumeReadOps, VolumeWriteOps, VolumeTotalReadTime, VolumeTotalWriteTime
  - VolumeIdleTime, VolumeQueueLength, VolumeThroughputPercentage, VolumeConsumedReadWriteOps, BurstBalance
- common events
  - createVolume, deleteVolume, attachVolume, reattachVolume, modifyVolume, createSnapshot, createSnapshots, copySnapshot, shareSnapshot
- Data is reported to CloudWatch only when the volume is attached to an instance.

## FSx

### lustre

## EventBridge

- CloudWatch sends events to Amazon EventBridge whenever a CloudWatch alarm changes alarm state
- use EventBridge and these events to write rules that take actions,

## neptune
