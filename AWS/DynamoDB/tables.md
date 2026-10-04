# Tables

> Tables. Part of the [DynamoDB](../) cheatsheet.

## To create a table on demand

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

## To create a provisioned table

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

## To wait for a table to become usable

The following example blocks until the table is active, which saves you from
writing to a table that is still creating.

```bash
aws dynamodb wait table-exists --table-name <table>
```

## To create a table with a composite key

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

## To list tables

The following example returns the table names in one region, and the call is
per region, so repeat it per region to see a whole account.

```bash
aws dynamodb list-tables --region <region>
```

## To list tables one page at a time

The following example returns ten names and stops there, because the CLI pages
the whole list for you unless you cap it.

```bash
aws dynamodb list-tables --max-items 10
```

## To continue from the previous page

The following example resumes from the token the previous page returned, where
the token for this call is the last table name it printed.

```bash
aws dynamodb list-tables --starting-token <last-table-name>
```

## To describe a table

The following example returns the key schema, item count, and billing mode
for a table, which is the first call to make when a `query` behaves oddly.

```bash
aws dynamodb describe-table --table-name <table>
```

## To switch a table to on demand

The following example moves a provisioned table onto on-demand billing
immediately, with no downtime.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --billing-mode PAY_PER_REQUEST
```

## To scale provisioned throughput

The following example raises read and write capacity, and the change is
asynchronous, so wait for the table to become active again.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --provisioned-throughput \
        ReadCapacityUnits=100,WriteCapacityUnits=100
```

## To turn on deletion protection

The following example makes `delete-table` fail until you turn it off, which
is the closest thing DynamoDB has to a dry run for a table.

```bash
aws dynamodb update-table \
    --table-name <table> \
    --deletion-protection-enabled
```

## To tag a table

The following example adds tags to a table, which need its ARN rather than
its name.

```bash
aws dynamodb tag-resource \
    --resource-arn arn:aws:dynamodb:<region>:<account-id>:table/<table> \
    --tags Key=Environment,Value=prod Key=Owner,Value=platform
```

## To list the tags on a table

The following example returns the tags on a table.

```bash
aws dynamodb list-tags-of-resource \
    --resource-arn arn:aws:dynamodb:<region>:<account-id>:table/<table>
```

## To delete a table

The following example deletes a table and every item in it. The API has no
`DryRun` parameter, so take a backup first, confirm the item count with
`describe-table`, and remember that recovery means restoring from that backup
rather than waiting.

```bash
aws dynamodb delete-table --table-name <table>
```

## To wait for a table to disappear

The following example blocks until the delete finishes, so a follow up
`create-table` with the same name does not collide with it.

```bash
aws dynamodb wait table-not-exists --table-name <table>
```
