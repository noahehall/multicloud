# cli S3

## links

- [high level cmds](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html)
- [low level cmds](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-apicommands.html)

## basics

- high level cmds: enable you to manage the contents of Amazon S3 within itself and with local directories
- low level cmds: provide direct access to the Amazon S3 APIs, enabling operations not exposed in the high-level s3 commands.
  - are generated from JSON models that directly imitate the APIs of other AWS services that provide API-level access.
  - allow a significantly more granular amount of control over your buckets when using the CLI.

```sh
##################### high level cmds
aws s3 X Y Z

mb s3://SOME_BUCKET # create SOME_BUCKET
cp SOME_FILE s3://SOME_BUCKEt # cp SOME_FILE to SOME_BUCKET
ls # list all buckets
  SOME_BUCKET # list all objects in bucket

##################### low level cmds
aws s3api X Y Z

list-objects
make-bucket
```
