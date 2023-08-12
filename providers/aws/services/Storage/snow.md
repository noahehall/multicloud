# Snow

- physical edge computing & storage devices for running operations in harsh, non-data center environments, locations with inconsistent network connectivity, or transporting exabytes of data into and out of AWS
- stuff in this file generally applies to all snow family devices
  - [snowcone](./snow-snowcone.md)
  - [snowball](./snow-snowball.md)
  - [snowmobile](./snow-snowmobile.md)

## my thoughts

## links

- [landing page](https://aws.amazon.com/snow/?did=ap_card&trk=ap_card)
- [OpsHub GUI](https://docs.aws.amazon.com/snowball/latest/developer-guide/aws-opshub.html)

## best practices

### anti patterns

## features

- lease Purpose-built devices to cost effectively move petabytes of data, offline
- for the most extreme conditions, delivering high security and ruggedization into compute and storage-compatible devices.
- for space- or weight-constrained environments, portability, and flexible networking options.

### pricing

- rental fee for the duration of the device
- ondemand: for shorter-term rentals
  - includes service fee per job
- committed: for longer-term rentals

## basics

- Each device is designed to meet the unique compute and capacity challenges required by different use cases.
  - help physically transport up to exabytes of data into and out of AWS.
  - AWS owns the devices, you lease them
- snowcone: A small, rugged, portable, secure edge computing, storage, and data transfer device.
- snowball: A rugged petabyte-scale data transport device with onboard storage and compute capabilities.
  - different models of AWS Snowball Edge devices to choose from.
- Snowmobile: A large truck to migrate or transport exabyte-scale datasets into and out of the AWS Cloud.

### Job Types

- the device (snowball vs snowcone), device configuration and job type determines feature matrix
  - you need to check the docs and walkthrough the decision tree

#### Import to Amazon S3

- shorter duration use cases where you also want the data imported into Amazon S3
- ship an empty Snowball Edge device to you
- return the device and your data is uploaded to your Amazon S3 bucket selected during job creation
- Snow service
  - Snowball Edge only as a single device. Clustering the device is not permitted for import jobs.
    - storage Optimized without compute
      - use the device only for transferring data
    - Storage Optimized with compute and Compute Optimized devices
      - edge data collection and data processing
      - ability import the data into Amazon S3 when the device is returned to AWS.
  - Snowcone
    - pre-formatted as mostly NFS storage
    - additional space is available for Amazon EBS volumes to support the onboard Amazon EC2 instances.
    - DataSync client is not preloaded with import to Amazon S3 jobs.

#### Export from Amazon S3

- AWS loads data from your Amazon S3 bucket on the device and ships it to you.
- copy data from your device to your local storage. then ship the device back
- Snow Service
  - Snowball/Snowcone storage optimized without compute
    - export your data from your chosen Amazon S3 bucket.
    - only allows for exporting your data to your on-premises storage location

#### Local compute and storage

- longer duration edge compute and storage use cases,
- no data is imported into Amazon S3 when the device is returned to AWS.
- perform compute and storage workloads on the device without any data transferred into AWS when returned
- AWS erases all data on the device securely.
- Snow service
  - Snowcone
  - use the preloaded DataSync client to import the data into your AWS storage services.

### OpsHub

- downloadable tool for managing snow devices
- takes all the existing operations available in the Snowball API and presents them as a graphical user interface.

### Data Transfer

- you ship the device back to a specific AWS region
  - bunch of tracking options are available
- S3 buckets where data will be imported are set up before you order the device.

### Security

- Trusted Platform Module: TPM; provides a hardware root of trust.
  - interface to the trusted software stack during the measurements and verification of the boot environment integrity.
  - Verification is performed after the power is switched on and before the Snowball Edge device is ready to be used.
- data erasure after transfer completion and device returned follows the National Institute of Standards and Technology (NIST) guidelines for media sanitization.

## considerations

## integrations
