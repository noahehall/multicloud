# File System X (FSx)

- native compatibility with third party file systems that uses NFS access protocol.
- applications need to access shared files and require a file system

## my thoughts

## links

- [landing page](https://aws.amazon.com/fsx/?did=ap_card&trk=ap_card)

## best practices

### anti patterns

## features

### pricing

- depends on the underlying type
  - FSx for NetApp ONTAP
  - FSx for OpenZFS
  - FSX for windows file server
  - FSx for Lustre

## terms

## basics

### Lustre file system

- designed for high performance computing (HPC) and machine learning (ML) workloads. FSx for Lustre uses the Lustre client's POSIX-compliant access protocol.

### Windows File Server

- The access protocol is Server Message Block (SMB) and designed for your Microsoft applications and Windows workloads.

### NetApp ONTAP

- designed to provide both NetApp block and file storage. The access protocols are iSCSI for block storage, NFS and SMB for file storage.

### OpenZFS

- fully managed shared file storage
- migrate your on-premises OpenZFS storage to AWS with minimal effort. You can use the same access protocols now in the AWS Cloud.

## considerations

## integrations
