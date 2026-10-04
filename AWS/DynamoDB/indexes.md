# Indexes

> Indexes. Part of the [DynamoDB](../) cheatsheet.

## To create a table with a global secondary index

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

## To add a global secondary index to an existing table

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

## To add a local secondary index

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

## To list the indexes on a table

The following example returns every secondary index with its key schema and
throughput, which is where you check whether an index is still creating.

```bash
aws dynamodb describe-table --table-name <table> \
    --query 'Table.[GlobalSecondaryIndexes,LocalSecondaryIndexes]'
```

## To query a global secondary index

The following example queries the index rather than the table, so the
`key-condition-expression` must name the index's own key attributes.

```bash
aws dynamodb query \
    --table-name <table> \
    --index-name <index> \
    --key-condition-expression "<index-partition-key> = :pk" \
    --expression-attribute-values '{":pk":{"S":"<value>"}}'
```

## To delete a global secondary index

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
