# Cloudwatch Integrations, Metrics and Events

- catchall for
  - integratin patterns
  - common metrics and events for various AWS services

## links

- [backup: metrics](https://docs.aws.amazon.com/aws-backup/latest/devguide/cloudwatch.html)
- [dynamodb: alarms](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/creating-alarms.html)
- [dynamodb: contributor insights tut](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/contributorinsights_tutorial.html)
- [dynamodb: contributor insights](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/contributorinsights_HowItWorks.html)
- [dynamodb: metrics & dimensions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/metrics-dimensions.html)
- [dynamodb: monitoring](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/monitoring-cloudwatch.html)
- [ebs: metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cloudwatch_ebs.html)
- [ebs: metrics for nitro instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html#ebs-metrics-nitro)
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
- [neptune: cloudwatch logs](https://docs.aws.amazon.com/neptune/latest/userguide/cloudwatch-logs.html)
- [neptune: monitoring](https://docs.aws.amazon.com/neptune/latest/userguide/cloudwatch.html)
- [neptune: metrics](https://docs.aws.amazon.com/neptune/latest/userguide/cw-metrics.html)
- [s3: metrics](https://docs.aws.amazon.com/AmazonS3/latest/userguide/metrics-dimensions.html)
- [sns: metrics](https://docs.aws.amazon.com/sns/latest/dg/sns-monitoring-using-cloudwatch.html)
- [sns: setup](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html)
- [sqs: metrics](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html)
- [step functions: metrics](https://docs.aws.amazon.com/step-functions/latest/dg/procedure-cw-metrics.html)
- [storageGateway: file gateway metrics](https://docs.aws.amazon.com/filegateway/latest/files3/monitoring-file-gateway.html)
- [storageGateway: volume gateway metrics](https://docs.aws.amazon.com/storagegateway/latest/vgw/monitoring-volume-gateway.html)
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
- burstable metrics
  - Baseline utilization %: (accrued credits/number of vCPUs)/60 minutes
  - CPUCreditUsage: CPU credits spent during the measurement period.
  - CPUCreditBalance: CPU credits that an instance has accrued. This balance is depleted when the CPU bursts and CPU credits are spent more quickly than they are earned.
  - CPUSurplusCreditBalance: The number of surplus CPU credits spent to sustain CPU utilization when the - CPUCreditBalance value is zero.
  - CPUSurplusCreditsCharged: The number of surplus CPU credits exceeding the maximum number of CPU credits that can be earned in a 24-hour period, thus attracting an additional charge.
- common alarms
  - when your burstable credits fall below a specific threshold.

### detailed monitoring

- apps running on EC2 can post metrics every minute (instead of every 5 with basic monitoring)

### enhanced networking

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

- Data is reported to CloudWatch only when the volume is attached to an instance.
  - some metrics are not reported in multi attach scenarios
- consider also installing the cloudwatch Agent on the attached EC2 instances to gather low-level performance metrics
  - use these disk-specific metrics to check on the free space, total space used, and total space available.
- important metrics
  - VolumeIdleTime: when trying to identify volumes that are unused or underused.
  - VolumeQueueLength: identify bottlenecks related to a specific EC2 instance type.
  - BurstBalance: monitors the balance of burst buckets for gp2 volumes.
- common metrics
  - bandwidth, throughput, latency, VolumeReadBytes, VolumeWriteBytes, VolumeReadOps, VolumeWriteOps, VolumeTotalReadTime, VolumeTotalWriteTime
  - VolumeThroughputPercentage, VolumeConsumedReadWriteOps
- troubleshooting metrics
  - check i/o burst balance
  - Estimates the IOPS requirement by workload
    - Find your primary data volume's VolumeReadOps and VolumeWriteOps metrics
    - Use the CloudWatch Sum statistic to get the peak levels of VolumeReadOps and VolumeWriteOps
      - volume read ops + volume write ops
    - estimate how many IOPS you need
      - divide the total from the previous step by the number of seconds in the measurement interval
      - e.g. 936,000 / 5 minutes (or 300 seconds) = 3,120 IOPS
- agent metrics
  - `disk_{total,used,used_percent,free}`

### Micro-bursting

- occurs when an EBS volume "bursts" high IOPS or throughput for significantly shorter periods than the collection period
- CloudWatch monitors EBS volumes' IOPS (op/s) and throughput (byte/s) by collecting samples every minute for most volumes
  - thus your application appears to be throttle but cloudwatch isnt reporting as such because it cant see it
  - Because the volume bursts high IOPS or throughput for a shorter time than the collection period
- remediation
  - Monitor CloudWatch's VolumeIdleTime metric to identify possible micro-bursting.
  - Confirm micro-bursting using an OS-level tool, such as iostat.
  - Prevent micro-bursting by changing your volume size or type to accommodate your applications.

## FSx

### lustre

## EventBridge

- CloudWatch sends events to Amazon EventBridge whenever a CloudWatch alarm changes alarm state
- use EventBridge and these events to write rules that take actions,

## neptune

## s3

- request metrics: api request level stuff
  - available at 1 minute intervals
  - billed at the same rate as custom metrics
- replication metrics: pending replication, total size pending, max replication time
- daily storage metrics: total & size of objects sans S3 glacier class
  - free and enabled by default
  - avalable in a report once per day

## Backup

- monitor AWS Backup metrics by using the aws/backup namespace.
- category
  - jobs: e.g. Monitor the number of failed backup jobs within one or more specific backup vaults
    - metrics: Number of backup, restore, and copy jobs across each state
    - dimensions: resource type, vault name
  - Recovery points: e.g. Track the number of deleted and warm and cold recovery points in each backup vault.
    - metrics: Number of warm and cold recovery points across each state
    - dimensions: Resource type, vault name

## storage gateway

- AvailabilityNotifications Number of availability-related notifications sent by the volume.
- CacheHitPercent Percent of application read operations from the volume that are served from cache.
- CachePercentDirty The volume's contribution to the overall percentage of the gateway's cache that isn't persisted to AWS.
- CachePercentUsed The volume's contribution to the overall percent use of the gateway's cache storage.
- CloudBytesUploaded The total number of bytes that the gateway uploaded to AWS during the reporting period.
- CloudBytesDownloaded The total number of bytes that the gateway downloaded from AWS during the reporting period.
- HealthNotifications The number of health notifications sent by the volume.
- IoWaitPercent Percent of time that the gateway is waiting on a response from the local disk.
- ReadBytes The total number of bytes read from your on-premises applications in the reporting period.
- WriteBytes The total number of bytes written to your on-premises applications in the reporting period.
- important metrics:
  - Analyze the CloudBytesDownloaded and CloudBytesUploaded metrics to understand throughput between your gateway and the AWS Cloud.
  - Use the ReadTime and WriteTime metrics with the Average statistic to measure latency.
  - Monitor performance of your Storage Gateway by monitoring metrics like CachePercentDirty.
    - The higher the percentage of dirty cache > the lower the space available for low-latency data.
  - Use the ReadBytes and WriteBytes metrics with the Sum statistic to measure throughput from your on-premises applications coming into the gateway.
- important alarms:
  - High IO wait: IoWaitPercent >= 20 for 3 datapoints in 15 minutes
  - Cache percent dirty: CachePercentDirty > 80 for 4 datapoints within 20 minutes
  - Availability notifications: AvailabilityNotifications >= 1 for 1 datapoints within 5 minutes
  - Health notifications: HealthNotifications >= 1 for 1 datapoints within 5 minutes
