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

- parallel file system
- designed for high performance computing (HPC) and machine learning (ML) workloads. FSx for Lustre uses the Lustre client's POSIX-compliant access protocol.

### Windows File Server

- file system for Windows environments
- supports the Server Message Block (SMB) protocol.

### NetApp ONTAP

- designed to provide both NetApp block and file storage. The access protocols are iSCSI for block storage, NFS and SMB for file storage.

### OpenZFS

- fully managed shared file storage
- supports NFS and SMB protocols for a wide range of application implementations.

## considerations

## integrations
