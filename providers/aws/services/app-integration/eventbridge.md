# EventBridge fka CloudWatch Events

- serverless event bus service that you can use to connect your applications with data from a variety of sources

## links

- [landing page](https://aws.amazon.com/eventbridge/?did=ap_card&trk=ap_card)
- [user guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)
- [content filtering with event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/content-filtering-with-event-patterns.html)
- [architectural patterns with eventbridge pipes](https://aws.amazon.com/blogs/compute/implementing-architectural-patterns-with-amazon-eventbridge-pipes/)
- [backup: events](https://docs.aws.amazon.com/aws-backup/latest/devguide/eventbridge.html)
- [storageGateway: processing events](https://aws.amazon.com/blogs/storage/processing-file-upload-notifications-from-aws-storage-gateway-on-amazon-s3/)

## best practices

### anti patterns

## features

- Create point-to-point integrations between event producers and consumers
- Connect AWS services, software-as-a-service (SaaS) applications, and custom applications as event producers to launch workflows.
  - 90+ services as sources, 20+ services as targets
- Create, trigger, and manage millions of events and tasks from a single source with Amazon EventBridge Scheduler.
- help manage errors across services

### pricing

- pay for events published to your event bus, and events ingested for Schema Discovery and event replay
  - $1 per million events in the bus, no charge for delivery

## terms

## basics

### events

- an indicator of a change in an AWS resource
- are represented as JSON objects. The fields that are unique to the event are contained in the "detail" section of the JSON object. The "event" field contains the event name. The "result" field contains the completed status of the action that triggered the event.
- events can be based on service, schedule, or an alarm

#### rules

- match events and route them to one/more target functions or streams

### event bus

- has rules and filters for sending messages to appropriate consumers

### producers

### consumers

## integrations

### cloudwatch

- when metrics are logged to cloudwatch, an Event can be configured to deliver near real-time streams that describe changes in aws resources
- you create an event in order to perform an action when that event occurs

### Backup

- Backup sends events to EventBridge in a best-effort manner every 5 minutes.
- EventBridge tracks more changes than the Backup notification API, including changes to backup vaults, copy job state, Region settings, and the number of cold or warm recovery points.

### EBS

- EBS sends notifications to Amazon EventBridge for a variety of changes to the volume, snapshot, and encryption status
- establish rules that trigger programmatic actions in response to a change in volume, snapshot, or encryption key state
- important events
  - createVolume
  - deleteVolume
  - {re}attachVolume
- common events
  - modifyVolume, createSnapshot, createSnapshots, copySnapshot, shareSnapshot
