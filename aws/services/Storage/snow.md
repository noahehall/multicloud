# Snow

- physical edge computing & storage devices for running operations in harsh, non-data center environments and in locations with inconsistent network connectivity
- [snowcone](./snow-snowcone.md)
- [snowball](./snow-snowball.md)
- [snowmobile](./snow-snowmobile.md)

## my thoughts

## links

- [landing page](https://aws.amazon.com/snow/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

- lease Purpose-built devices to cost effectively move petabytes of data, offline
- for the most extreme conditions, delivering high security and ruggedization into compute and storage-compatible devices.
- for space- or weight-constrained environments, portability, and flexible networking options.

### pricing

## basics

- Each device is designed to meet the unique compute and capacity challenges required by different use cases.
  - help physically transport up to exabytes of data into and out of AWS.
  - AWS owns the devices, you lease them
- snowcone: A small, rugged, portable, secure edge computing, storage, and data transfer device.
- snowball: A rugged petabyte-scale data transport device with onboard storage and compute capabilities.
  - different models of AWS Snowball Edge devices to choose from.
- Snowmobile: A large truck to migrate or transport exabyte-scale datasets into and out of the AWS Cloud.

### Job Types

#### Import to Amazon S3

- shorter duration use cases where you also want the data imported into Amazon S3
- ship an empty Snowball Edge device to you
- return the device and your data is uploaded to your Amazon S3 bucket selected during job creation

#### Export from Amazon S3

- AWS loads data from your Amazon S3 bucket on the device and ships it to you.
- copy data from your device to your local storage. then ship the device back

#### Local compute and storage

- longer duration edge compute and storage use cases,
- perform compute and storage workloads on the device without any data transferred into AWS when returned
- AWS erases all data on the device securely.

## considerations

## integrations
