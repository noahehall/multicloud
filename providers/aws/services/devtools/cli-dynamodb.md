# CLI - dynamodb

## links

- [list-tables](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/dynamodb/list-tables.html)
- [get-item](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/dynamodb/get-item.html)
- [describe-limits](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/dynamodb/describe-limits.html)

## basics

```sh

dynamodb X Y Z

list-tables # table names
describe-table # bunches of stuff
  --table-name SUM_TABLE
get-item # matching the primary key in SOME.json
  --table-name SUM_TABLE
  --key file://SOME.json
  --return-consumed-capacity TOTAL # use TOTAL keyword
describe-limits # provisioned-capacity quotas for your AWS account in a Region
```
