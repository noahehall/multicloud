# service name

- Store and access SMB and NFS file data with local caching
- S3 and FSx file gateways

## my thoughts

## links

- [landing page](https://aws.amazon.com/storagegateway/file/?nc=sn&loc=2&dn=2)
- [s3: intro](https://aws.amazon.com/storagegateway/file/s3/)
- [fsx: intro](https://aws.amazon.com/storagegateway/file/fsx/)

## best practices

### anti patterns

## features

### pricing

## basics

### s3 file gateway

- used to store NFS and SMB files as s3 objects for data lakes, backups, and ML workflows.
- accessbile over NFS, SMB and or directly via s3 API
- POSIX-style metadata: including ownership, permissions, and timestamps, are durably stored in Amazon S3 in the user-metadata of the object associated with the file.
- objects transferred to S3, are managed as native S3 objects.

### FSx File gateway

- provides low-latency, on-premises access to fully managed file shares in FSx Windows File Server.
  - store and access file data with Windows-native compatibility, including full NTFS support, shadow copies, and access control lists (ACLs).
- user or team file shares, and file-based application migrations like web content management, and media workflows.
- migrate and consolidate on-premises file-based application data stored on NAS arrays or file server virtual machines into FSx

## considerations

## integrations

### Backup

### Cloudwatch
