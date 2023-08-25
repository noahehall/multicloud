# Dynamodb API

## links

- [api retries](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)
- [batch get item](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_BatchGetItem.html)
- [batch read writem](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_BatchWriteItem.html)
- [change data capture](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/streamsmain.html)
- [pagination](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.Pagination.html)
- [query (guide)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html)
- [query (ref)](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Query.html)
- [Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html)

## basics

- in general
  - batchBLAH is more efficient than multiple non batches
  - deleteBLAH costs the same WCU as creation
  - scan should be avoided unless required, and effectively filtered as to not read the entire table
    - even when filtered, you are stilled charged for the total amount retrieved before filtering
    - every item is scanned in the table inorder to build the resultset
  - query can only be used on tables with partitioned composite keys
    - fully indexed and very fast (relative to scan)
- writes
  - putItem: upsert single item
  - updateItem: upsert item attributes
  - batchWriteItem: upsert multiple items
  - deleteItem: single item; costs the same amount of WCU as putItem
- reads
  - getItem: single item
  - batchGetItem: multiple items
  - query: retrieve items matching sort key expression for specific partition
  - scan: retrieve items across all partitions in table
- client behavior and configuration
  - error handling:
    - 500 errors can be retried, e.g. Provisioned THroughput exceeded (throttling)
    - 400 errors need to be resolved on the client: e.g. required params missing
    - batch operations
      - operate as loops around get/put/delete items
      - individual requests in the loop which do not complete are returned as such
        - you can implement your own retry loop with exponential-backoff
  - tuning retries
    - set a max number of retries, times and strategies for exponential back-off & jitter
