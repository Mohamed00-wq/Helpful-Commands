# Batch Operations

> Batch Operations. Part of the [DynamoDB](../) cheatsheet.

A batch call sends many items in one request: up to 100 keys to read, up to 25
writes, and a provisioned table counts the whole batch as a single write.

## To read several items at once

The following example reads up to 100 keys across tables in one request, and
a key that does not exist is simply absent from `Responses` rather than an
error.

```bash
aws dynamodb batch-get-item \
    --request-items file://batch-get.json
```

`batch-get.json`:

```json
{
  "<table>": {
    "Keys": [
      { "<partition-key>": { "S": "<value-1>" } },
      { "<partition-key>": { "S": "<value-2>" } }
    ],
    "ConsistentRead": true
  }
}
```

## To write several items at once

The following example writes and deletes items in one request, and the payload
is a map of table name to a list of `PutRequest` and `DeleteRequest` entries.

```bash
aws dynamodb batch-write-item \
    --request-items file://batch-write.json
```

`batch-write.json`:

```json
{
  "<table>": [
    {
      "PutRequest": {
        "Item": { "<partition-key>": { "S": "<value-1>" } }
      }
    },
    {
      "DeleteRequest": {
        "Key": { "<partition-key>": { "S": "<value-2>" } }
      }
    }
  ]
}
```

A batch write has no condition expressions. Unprocessed items are returned and
must be retried, because a partial batch is a normal outcome.
