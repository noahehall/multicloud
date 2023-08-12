# Snowmobile

- exabyte-scale data transfer service used to move large amounts of data to AWS
- move 100 PB of data in as little as a few weeks, plus transport time.
  - That same transfer could take more than 20 years to accomplish over a direct connect line with a 1-Gbps connection.

## my thoughts

## links

- [landing page](https://aws.amazon.com/snowmobile/)

## best practices

### anti patterns

## features

- transfer up to 100 petabytes of data, a 45-foot long ruggedized shipping container, pulled by a semitrailer truck
- retrieve data from the cloud whenever you need it.
- customizable, physically transferred data security with 24/7 video surveillance, tamper-resistant hardware, GPS tracking, data encryption, and optional security personnel.
  - Customize data transfer operations to your location

### pricing

## basics

### Security

- AWS Snowmobile is tamper-resistant, water-resistant, and temperature controlled.
- The data container is operated only by AWS personnel.
- Physical access is limited using security access hardware controls.
- protected by 24/7 video surveillance and alarm monitoring, GPS tracking, and may be escorted by a security vehicle while in transit.

#### Encryption

- data is encrypted with keys that you provide before it is written to Snowmobile.
  - Encryption keys are never written to disk.
  - Should power be removed for any reason, the keys are securely erased.
- manage your encryption keys with KMS
-

## considerations

## integrations
