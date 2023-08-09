# Snowcone

- Deploy ultra-portable data transfer and edge computing devices anywhere
- The smallest snow device about the size of a tissue box and preferred for IoT usecases

## my thoughts

## links

- [landing page](https://aws.amazon.com/snowcone/)

## best practices

### anti patterns

## features

- transmit crucial data in real time with built-in Wi-Fi and AWS DataSync. or ship the device with data to AWS for offline transfer.
- collect terabytes of data daily from large vehicle fleets in a small form factor
- Gather/Deploy an IoT/edge computing and storage solution for environments with limited space, bandwidth, and harsh conditions such as factories, mines, and oil fields.
- compute, storage, and network accessibility you need in a portable device, deployed virtually anywhere.
- meets various ruggedization standards

### pricing

- pay only for the use of the device and for data transfer out of AWS
- service fee per job, includes five days of device use and a per-day fee each additional day
  - Snowcone
  - Snowcone SSD

## basics

### Onboard compute

- runs EC2 instances
  - 2 vCPUs
  - 4 GB memory
- select the appropriate AMI when you order

### Online Data Transfer

- DataSync comes pre-installed
  - import/export data to/from s3, efs, FSx,

### File-based storage

- 8 TB of usable storage accessible as an NFS mountpoint

### Security

- meets ruggedization standards: ISTA-3A, ASTM D4169, and MIL-STD-810G for free-fall shock, operational vibration; dust and water jets; freezing and hot/desert tempuratures; designed to tolerate falls up to 3.8 feet (1.15 meters).

## considerations

## integrations

### GreenGrass

### EC2
