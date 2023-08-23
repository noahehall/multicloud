# Kinesis: Data Streams

- serverless streaming data service that makes it easy to elastically ingest and store logs, events, clickstreams, and other forms of streaming data in real time

## my thoughts

## links

- [landing page](https://aws.amazon.com/kinesis/data-streams/)

## best practices

- on demand vs provisioned
  - provisioned: best if you have predictable application traffic, run applications whose traffic is consistent or ramps gradually, or can forecast capacity requirements to control costs.

### anti patterns

## features

- automatic provisioning and scaling with the on-demand mode.
- built-in integrations with other AWS services to create analytics, serverless, and application integration solutions
- Stream log and event data: Ingest and collect terabytes of data per day from application and service logs, clickstream data, sensor data, and in-app user events
- Run real-time analytics: high-frequency event data such as clickstream data, and gain access to insights in seconds,
- Power event-driven applications

### pricing

- on-demand capacity mode:
  - pay per GB of data written and read from your data streams
  - each stream in your account at an hourly rate.
  - additional charges for optional features:
    - Extended data retention
    - Long-Term data retention
    - Enhanced Fan-Out
- provisioned
  - charged for each shard at an hourly rate.

## basics

- Video Streams: stream video from connected devices into aws for analytics, ML, playback and other processing
- Data Streams: serverless streaming service; capture, `process` and store data streams
- you need to configure the amount of shards per stream

### Capacity Modes

#### On Demand

- you dont specify how much read and write throughput
- instantly accommodates your workloads as they ramp up or down.

#### Provisioned

- specify the number of shards necessary for your application based on its write and read request rate.
- shard: a unit of capacity that
  - 1 MB/second of write
  - reads: up to 2 MB/second of read throughput
    - without enhanced fanout: shared for ALL consumers
    - with enhanced fan-out: for EACH consumer

## considerations

## integrations
