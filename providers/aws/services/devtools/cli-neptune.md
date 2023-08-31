# CLI : Neptune

## links

- [neptune cli docs](https://docs.aws.amazon.com/cli/latest/reference/neptune/index.html#cli-aws-neptune)

## basics

```sh
# create a neptune cluster
aws neptune create-db-cluster \
  --db-cluster-identifier sample-cluster-cli \
  --engine neptune

# create db instance
aws neptune create-db-instance \
  --db-cluster-identifier sample-cluster-cli \
  --db-instance-identifier sample-db-instance \
  --db-instance-class db.t3.medium \
  --engine neptune

# db instance status
aws neptune describe-db-instances \
  --db-instance-identifier sample-db-instance

# db instance delete
aws neptune delete-db-instance \
  --db-instance-identifier sample-db-instance \
  --skip-final-snapshot

# db cluster delete

aws neptune delete-db-cluster \
  --db-cluster-identifier sample-cluster-cli \
  --skip-final-snapshot
```
