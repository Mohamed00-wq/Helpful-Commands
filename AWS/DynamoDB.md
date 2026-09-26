# DynamoDB (NoSQL Database)

> Commands for tables, keys, items, condition expressions, secondary indexes,
> time to live, and the pagination that `query` and `scan` both require.

A `query` reads one partition and stops at its sort key, so it costs a fraction
of a `scan`. A `scan` reads the whole table, every page of it, and you pay for
every item it looked at even when the filter throws it away. `scan` is for
backups, counts, and migrations you cannot express as a key condition; for
everything else, add an index and `query` it.

`query` and `scan` both return at most 1 MB per call. A table larger than that
needs several calls, which is what the Pagination section below is for.

## Tables

### To create a table on demand

The following example creates a table billed per request rather than per
provisioned unit, which is the right default for spiky or unknown traffic.

```bash
aws dynamodb create-table \
    --table-name <table> \
    --attribute-definitions file://attribute-definitions.json \
    --key-schema file://key-schema.json \
    --billing-mode PAY_PER_REQUEST \
    --tags Key=Environment,Value=prod
```

`attribute-definitions.json`:

```json
[
  { "AttributeName": "<partition-key>", "AttributeType": "S" }
]
```

`key-schema.json`:

```json
[
  { "AttributeName": "<partition-key>", "KeyType": "HASH" }
]
```

### To create a provisioned table

The following example creates a table with fixed read and write capacity,
which is cheaper than on demand only when the load is steady.

```bash
aws dynamodb create-table \
    --table-name <table> \
    --attribute-definitions file://attribute-definitions.json \
    --key-schema file://key-schema.json \
    --provisioned-throughput \
        ReadCapacityUnits=5,WriteCapacityUnits=5
```

### To wait for a table to become usable

The following example blocks until the table is active, which saves you from
writing to a table that is still creating.

```bash
aws dynamodb wait table-exists --table-name <table>
```

### To create a table with a composite key

The following example adds a sort key, and a query can only ever filter on the
partition key, never on a non-key attribute.

```bash
aws dynamodb create-table \
    --table-name <table> \
    --attribute-definitions file://attribute-definitions.json \
    --key-schema file://key-schema.json \
    --billing-mode PAY_PER_REQUEST
```

`attribute-definitions.json`:

```json
[
  { "AttributeName": "<partition-key>", "AttributeType": "S" },
  { "AttributeName": "<sort-key>", "AttributeType": "S" }
]
```

`key-schema.json`:

```json
[
  { "AttributeName": "<partition-key>", "KeyType": "HASH" },
  { "AttributeName": "<sort-key>", "KeyType": "RANGE" }
]
```

### To list tables

The following example returns the table names in one region, and the call is
per region, so repeat it per region to see a whole account.

```bash
aws dynamodb list-tables --region <region>
```

### To list tables one page at a time

The following example returns ten names and stops there, because the CLI pages
the whole list for you unless you cap it.

```bash
aws dynamodb list-tables --max-items 10
```

### To continue from the previous page

The following example resumes from the token the previous page returned, where
the token for this call is the last table name it printed.

```bash
aws dynamodb list-tables --starting-token <last-table-name>
```

### To describe a table

The following example returns the key schema, item count, and billing mode
for a table, which is the first call to make when a `query` behaves oddly.

```bash
aws dynamodb describe-table --table-name <table>
```

### To switch a table to on demand

The following example moves a provisioned table onto on-demand billing
immediately, with no downtime.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --billing-mode PAY_PER_REQUEST
```

### To scale provisioned throughput

The following example raises read and write capacity, and the change is
asynchronous, so wait for the table to become active again.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --provisioned-throughput \
        ReadCapacityUnits=100,WriteCapacityUnits=100
```

### To turn on deletion protection

The following example makes `delete-table` fail until you turn it off, which
is the closest thing DynamoDB has to a dry run for a table.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --deletion-protection-enabled
```

### To tag a table

The following example adds tags to a table, which need its ARN rather than
its name.

```bash
aws dynamodb tag-resource \
    --resource-arn arn:aws:dynamodb:<region>:<account-id>:table/<table> \
    --tags Key=Environment,Value=prod Key=Owner,Value=platform
```

### To list the tags on a table

The following example returns the tags on a table.

```bash
aws dynamodb list-tags-of-resource \
    --resource-arn arn:aws:dynamodb:<region>:<account-id>:table/<table>
```

### To delete a table

The following example deletes a table and every item in it. The API has no
`DryRun` parameter, so take a backup first, confirm the item count with
`describe-table`, and remember that recovery means restoring from that backup
rather than waiting.

```bash
aws dynamodb delete-table --table-name <table>
```

### To wait for a table to disappear

The following example blocks until the delete finishes, so a follow up
`create-table` with the same name does not collide with it.

```bash
aws dynamodb wait table-not-exists --table-name <table>
```

## Backups

### To take a backup before deleting

The following example captures an on-demand backup, which is the only way
back after `delete-table`.

```bash
aws dynamodb create-backup \
    --table-name <table> \
    --backup-name <backup>
```

### To list a table's backups

The following example returns the backups you can restore from, with their
status and expiry.

```bash
aws dynamodb list-backups --table-name <table>
```

### To export a table to S3

The following example exports a point in time snapshot as DynamoDB JSON,
which is the format to use for a long term copy in a bucket.

```bash
aws dynamodb export-table-to-point-in-time \
    --table-arn arn:aws:dynamodb:<region>:<account-id>:table/<table> \
    --s3-bucket <bucket> \
    --export-format DYNAMODB_JSON
```

## Items

Every value is an `AttributeValue` object rather than a bare value: a string is
`{"S":"x"}`, a number is `{"N":"1"}`, and the number itself is a string inside
the quotes.

### To put an item

The following example writes a complete item, replacing any item already at
that key.

```bash
aws dynamodb put-item \
    --table-name <table> \
    --item file://item.json
```

`item.json`:

```json
{
  "<partition-key>": { "S": "<value>" },
  "<sort-key>": { "S": "<value>" },
  "userId": { "S": "<user-id>" },
  "status": { "S": "active" },
  "createdAt": { "S": "2026-09-26T00:00:00Z" }
}
```

### To get an item

The following example reads one item by its full key, which is a single call
no matter how large the table is.

```bash
aws dynamodb get-item \
    --table-name <table> \
    --key file://key.json
```

`key.json`:

```json
{
  "<partition-key>": { "S": "<value>" },
  "<sort-key>": { "S": "<value>" }
}
```

### To get only the attributes you need

The following example returns two attributes instead of the whole item, which
is cheaper on tables with wide items.

```bash
aws dynamodb get-item \
    --table-name <table> \
    --key file://key.json \
    --projection-expression "userId, createdAt"
```

### To get an item with a strongly consistent read

The following example waits for the latest write, which costs twice as much
and is only needed when a read must never return a stale value.

```bash
aws dynamodb get-item \
    --table-name <table> \
    --key file://key.json \
    --consistent-read
```

### To update an item

The following example sets one attribute and atomically increments a counter,
and the increment is the safe way to count events without reading first.

```bash
aws dynamodb update-item \
    --table-name <table> \
    --key file://key.json \
    --update-expression 'SET #s = :status ADD visits :one' \
    --expression-attribute-names '{"#s":"status"}' \
    --expression-attribute-values '{":status":{"S":"active"},":one":{"N":"1"}}'
```

### To remove an attribute

The following example drops an attribute from the item with `REMOVE`, which is
the only way to delete a single attribute without rewriting the item.

```bash
aws dynamodb update-item \
    --table-name <table> \
    --key file://key.json \
    --update-expression 'REMOVE #s' \
    --expression-attribute-names '{"#s":"status"}'
```

### To delete an item and keep a copy

The following example deletes an item and returns it as it was, which is how
you record what was removed without a second read.

```bash
aws dynamodb delete-item \
    --table-name <table> \
    --key file://key.json \
    --return-values ALL_OLD
```

### To delete an item only if a condition holds

The following example deletes the item only when its status still matches, and
fails with `ConditionalCheckFailedException` otherwise.

```bash
aws dynamodb delete-item \
    --table-name <table> \
    --key file://key.json \
    --condition-expression '#s = :expected' \
    --expression-attribute-names '{"#s":"status"}' \
    --expression-attribute-values '{":expected":{"S":"archived"}}' \
    --return-values ALL_OLD
```

## Query and Scan

### To query a partition

The following example reads every item in one partition, and a `query`
without a key condition is rejected because the partition key is mandatory.

```bash
aws dynamodb query \
    --table-name <table> \
    --key-condition-expression "<partition-key> = :pk" \
    --expression-attribute-values '{":pk":{"S":"<value>"}}'
```

### To query a range of sort keys

The following example narrows a partition to a window of sort keys, and
`begins_with` and `BETWEEN` both work on the sort key.

```bash
aws dynamodb query \
    --table-name <table> \
    --key-condition-expression \
        "#pk = :pk AND #sk BETWEEN :lo AND :hi" \
    --expression-attribute-names \
        '{"#pk":"<partition-key>","#sk":"<sort-key>"}' \
    --expression-attribute-values \
        '{":pk":{"S":"<value>"},":lo":{"S":"<from>"},":hi":{"S":"<to>"}}'
```

### To query the newest items first

The following example reverses the sort key order and caps the result, which
is the "latest ten for this user" query.

```bash
aws dynamodb query \
    --table-name <table> \
    --key-condition-expression "#pk = :pk" \
    --expression-attribute-names '{"#pk":"<partition-key>"}' \
    --expression-attribute-values '{":pk":{"S":"<value>"}}' \
    --scan-index-forward false \
    --limit 10
```

### To query only a few attributes

The following example projects a subset of attributes across every returned
item, rather than fetching each one whole.

```bash
aws dynamodb query \
    --table-name <table> \
    --key-condition-expression "#pk = :pk" \
    --projection-expression "userId, #s" \
    --expression-attribute-names \
        '{"#pk":"<partition-key>","#s":"status"}' \
    --expression-attribute-values '{":pk":{"S":"<value>"}}'
```

### To scan a table

The following example reads every item in the table, and it will cost more the
larger the table is, so reach for `query` whenever a key condition will do.

```bash
aws dynamodb scan --table-name <table>
```

### To filter a scan

The following example applies the filter after the read, so you still pay for
every scanned item even though only some come back.

```bash
aws dynamodb scan \
    --table-name <table> \
    --filter-expression '#s = :status' \
    --expression-attribute-names '{"#s":"status"}' \
    --expression-attribute-values '{":status":{"S":"active"}}'
```

### To count items with a scan

The following example counts matching items without transferring any of them,
which is the cheapest way to size a table.

```bash
aws dynamodb scan \
    --table-name <table> \
    --filter-expression 'attribute_exists(#pk)' \
    --expression-attribute-names '{"#pk":"<partition-key>"}' \
    --select COUNT
```

### To scan in parallel

The following example splits the table into four segments, and running one
call per segment in parallel is how a full table read finishes in reasonable
time.

```bash
aws dynamodb scan \
    --table-name <table> \
    --segment 0 \
    --total-segments 4
```

## Conditions and Locking

### To create an item only if it does not exist

The following example fails rather than overwriting, which is how you make
`put-item` safe to retry.

```bash
aws dynamodb put-item \
    --table-name <table> \
    --item file://item.json \
    --condition-expression 'attribute_not_exists(<partition-key>)'
```

### To guard an update with a version check

The following example writes only if the stored version is still the one the
caller read, and returns `ConditionalCheckFailedException` on a lost race,
which is optimistic locking with no extra round trip.

```bash
aws dynamodb update-item \
    --table-name <table> \
    --key file://key.json \
    --update-expression 'SET #v = :next' \
    --condition-expression '#v = :expected' \
    --expression-attribute-names '{"#v":"version"}' \
    --expression-attribute-values \
        '{":next":{"N":"2"},":expected":{"N":"1"}}'
```

### To see why a condition failed

The following example returns the item as it looked when the condition check
failed, which saves a second `get-item` to find out why.

```bash
aws dynamodb delete-item \
    --table-name <table> \
    --key file://key.json \
    --condition-expression 'attribute_exists(<partition-key>)' \
    --return-values-on-condition-check-failure ALL_OLD
```

### To use an attribute whose name is a reserved word

The following example escapes `status`, which DynamoDB treats as a reserved
word and will not accept bare in an expression.

```bash
aws dynamodb update-item \
    --table-name <table> \
    --key file://key.json \
    --update-expression 'SET #s = :status' \
    --expression-attribute-names '{"#s":"status"}' \
    --expression-attribute-values '{":status":{"S":"active"}}'
```

### To return the item after a write

The following example returns the new or old item in the write's own response,
which saves a follow up read.

```bash
aws dynamodb put-item \
    --table-name <table> \
    --item file://item.json \
    --return-values ALL_NEW
```

## Batch Operations

A batch call sends many items in one request: up to 100 keys to read, up to 25
writes, and a provisioned table counts the whole batch as a single write.

### To read several items at once

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

### To write several items at once

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

## Indexes

### To create a table with a global secondary index

The following example adds a GSI at create time, which avoids the backfill of
existing items that adding one later has to do.

```bash
aws dynamodb create-table \
    --table-name <table> \
    --attribute-definitions file://attribute-definitions.json \
    --key-schema file://key-schema.json \
    --global-secondary-indexes file://gsi.json \
    --billing-mode PAY_PER_REQUEST
```

`gsi.json`:

```json
[
  {
    "IndexName": "<index>",
    "KeySchema": [
      { "AttributeName": "<index-partition-key>", "KeyType": "HASH" },
      { "AttributeName": "<index-sort-key>", "KeyType": "RANGE" }
    ],
    "Projection": { "ProjectionType": "ALL" }
  }
]
```

### To add a global secondary index to an existing table

The following example adds a GSI to a live table, and every attribute named
in the index key must already be declared in the table's attribute
definitions.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --attribute-definitions file://index-attribute-definitions.json \
    --global-secondary-index-updates file://gsi-create.json
```

`index-attribute-definitions.json`:

```json
[
  { "AttributeName": "<index-partition-key>", "AttributeType": "S" }
]
```

`gsi-create.json`:

```json
[
  { "Create": {
      "IndexName": "<index>",
      "KeySchema": [
        { "AttributeName": "<index-partition-key>", "KeyType": "HASH" }
      ],
      "Projection": { "ProjectionType": "ALL" }
  } }
]
```

### To add a local secondary index

The following example adds an LSI, which is only possible at create time
because an LSI shares the table's partition key and cannot be added later.

```bash
aws dynamodb create-table \
    --table-name <table> \
    --attribute-definitions file://attribute-definitions.json \
    --key-schema file://key-schema.json \
    --local-secondary-indexes file://lsi.json \
    --billing-mode PAY_PER_REQUEST
```

`lsi.json`:

```json
[
  {
    "IndexName": "<index>",
    "KeySchema": [
      { "AttributeName": "<partition-key>", "KeyType": "HASH" },
      { "AttributeName": "<index-sort-key>", "KeyType": "RANGE" }
    ],
    "Projection": { "ProjectionType": "ALL" }
  }
]
```

### To list the indexes on a table

The following example returns every secondary index with its key schema and
throughput, which is where you check whether an index is still creating.

```bash
aws dynamodb describe-table --table-name <table> \
    --query 'Table.[GlobalSecondaryIndexes,LocalSecondaryIndexes]'
```

### To query a global secondary index

The following example queries the index rather than the table, so the
`key-condition-expression` must name the index's own key attributes.

```bash
aws dynamodb query \
    --table-name <table> \
    --index-name <index> \
    --key-condition-expression "<index-partition-key> = :pk" \
    --expression-attribute-values '{":pk":{"S":"<value>"}}'
```

### To delete a global secondary index

The following example removes an index to stop paying its storage and
provisioned capacity, and only the index is deleted, not the table.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --global-secondary-index-updates file://gsi-delete.json
```

`gsi-delete.json`:

```json
[
  { "Delete": { "IndexName": "<index>" } }
]
```

## Time to Live

### To enable time to live

The following example turns on expiry on a timestamp attribute, which holds
epoch seconds as a number, because that is what DynamoDB compares against the
current time.

```bash
aws dynamodb update-time-to-live \
    --table-name <table> \
    --time-to-live-specification \
        Enabled=true,AttributeName=expiresAt
```

### To check the time to live status

The following example returns the attribute name and the current status,
which may take up to an hour to change after you enable it.

```bash
aws dynamodb describe-time-to-live --table-name <table>
```

### To disable time to live

The following example stops expiring items, and items already marked expired
are not restored.

```bash
aws dynamodb update-time-to-live \
    --table-name <table> \
    --time-to-live-specification \
        Enabled=false,AttributeName=expiresAt
```

## Pagination

`query` and `scan` return at most 1 MB per call and a `LastEvaluatedKey` while
more data remains. The AWS CLI paginates both automatically, so most scripts
never see a `LastEvaluatedKey` unless you pass `--no-paginate`.

### To read one page of a scan

The following example returns a single page of at most 100 items, which is
what you want when a full table read would be slow or expensive.

```bash
aws dynamodb scan \
    --table-name <table> \
    --limit 100 \
    --no-paginate
```

### To resume a scan from the last key

The following example continues from a page boundary, where the start key is
the `LastEvaluatedKey` map from the previous page and not a page number.

```bash
aws dynamodb scan \
    --table-name <table> \
    --limit 100 \
    --exclusive-start-key file://last-evaluated-key.json \
    --no-paginate
```

`last-evaluated-key.json`:

```json
{
  "<partition-key>": { "S": "<value>" },
  "<sort-key>": { "N": "<value>" }
}
```

### To cap how much an automatic scan reads

The following example lets the CLI page automatically but stops after 500
items, and prints the token needed to resume where it left off.

```bash
aws dynamodb scan \
    --table-name <table> \
    --max-items 500 \
    --query 'Items'
```

### To resume an automatic scan

The following example continues from the `NextToken` the previous command
printed, and repeated calls walk the whole table.

```bash
aws dynamodb scan \
    --table-name <table> \
    --max-items 500 \
    --starting-token <next-token> \
    --query 'Items'
```

### To page through a query

The following example pages a partition the same way, so a partition larger
than 1 MB is returned in full rather than truncated.

```bash
aws dynamodb query \
    --table-name <table> \
    --key-condition-expression "#pk = :pk" \
    --expression-attribute-names '{"#pk":"<partition-key>"}' \
    --expression-attribute-values '{":pk":{"S":"<value>"}}' \
    --max-items 500
```

## Workflows

### To create a table and write to it

The following steps create an on-demand table, wait for it, then write and
read one item. Wait for the table before writing, because an item written
during creation fails.

1. Create the table. Later steps only need its name, so each one repeats
   `--table-name <table>`.

   ```bash
   aws dynamodb create-table \
       --table-name <table> \
       --attribute-definitions file://attribute-definitions.json \
       --key-schema file://key-schema.json \
       --billing-mode PAY_PER_REQUEST \
       --tags Key=Environment,Value=dev
   ```

2. Wait for the table to become active. This blocks for you, so a failure
   here means the create step itself failed.

   ```bash
   aws dynamodb wait table-exists --table-name <table>
   ```

3. Write one item, guarding against a duplicate retry.

   ```bash
   aws dynamodb put-item \
       --table-name <table> \
       --item file://item.json \
       --condition-expression \
           'attribute_not_exists(<partition-key>)'
   ```

4. Read it back and check the item count, which confirms the write landed.

   ```bash
   aws dynamodb get-item \
       --table-name <table> \
       --key file://key.json
   aws dynamodb describe-table --table-name <table> \
       --query 'Table.ItemCount'
   ```

### To read a whole table into a file

The following example exports every item as JSON Lines, one object per line,
by piping the paginated scan straight into a file.

```bash
aws dynamodb scan --table-name <table> --query 'Items' \
  | jq -c '.[]' > all-items.jsonl

wc -l all-items.jsonl
```

The CLI paginator merges the `Items` array of every 1 MB page into one
response, so `jq` receives the whole table and writes one item per line.

### To delete a table safely

The following order takes a restore point, checks what is about to be lost,
deletes the table, and waits for the delete to finish.

```bash
# Take a restore point first; delete-table has no dry run
aws dynamodb create-backup \
    --table-name <table> --backup-name <backup>

# Confirm the size of what you are deleting
aws dynamodb describe-table --table-name <table> \
    --query 'Table.[ItemCount,TableSizeBytes]'

# Delete and wait
aws dynamodb delete-table --table-name <table>
aws dynamodb wait table-not-exists --table-name <table>

# Re-creating the table does not bring the items back, so restore the backup
aws dynamodb restore-table-from-backup \
    --target-table-name <table> \
    --backup-arn <backup-arn>
```
