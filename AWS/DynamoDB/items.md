# Items

> Items. Part of the [DynamoDB](../DynamoDB.md) cheatsheet.

Every value is an `AttributeValue` object rather than a bare value: a string is
`{"S":"x"}`, a number is `{"N":"1"}`, and the number itself is a string inside
the quotes.

## To put an item

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

## To get an item

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

## To get only the attributes you need

The following example returns two attributes instead of the whole item, which
is cheaper on tables with wide items.

```bash
aws dynamodb get-item \
    --table-name <table> \
    --key file://key.json \
    --projection-expression "userId, createdAt"
```

## To get an item with a strongly consistent read

The following example waits for the latest write, which costs twice as much
and is only needed when a read must never return a stale value.

```bash
aws dynamodb get-item \
    --table-name <table> \
    --key file://key.json \
    --consistent-read
```

## To update an item

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

## To remove an attribute

The following example drops an attribute from the item with `REMOVE`, which is
the only way to delete a single attribute without rewriting the item.

```bash
aws dynamodb update-item \
    --table-name <table> \
    --key file://key.json \
    --update-expression 'REMOVE #s' \
    --expression-attribute-names '{"#s":"status"}'
```

## To delete an item and keep a copy

The following example deletes an item and returns it as it was, which is how
you record what was removed without a second read.

```bash
aws dynamodb delete-item \
    --table-name <table> \
    --key file://key.json \
    --return-values ALL_OLD
```

## To delete an item only if a condition holds

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
