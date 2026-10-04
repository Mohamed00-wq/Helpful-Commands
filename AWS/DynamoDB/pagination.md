# Pagination

> Pagination. Part of the [DynamoDB](../) cheatsheet.

`query` and `scan` return at most 1 MB per call and a `LastEvaluatedKey` while
more data remains. The AWS CLI paginates both automatically, so most scripts
never see a `LastEvaluatedKey` unless you pass `--no-paginate`.

## To read one page of a scan

The following example returns a single page of at most 100 items, which is
what you want when a full table read would be slow or expensive.

```bash
aws dynamodb scan \
    --table-name <table> \
    --limit 100 \
    --no-paginate
```

## To resume a scan from the last key

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

## To cap how much an automatic scan reads

The following example lets the CLI page automatically but stops after 500
items, and prints the token needed to resume where it left off.

```bash
aws dynamodb scan \
    --table-name <table> \
    --max-items 500 \
    --query 'Items'
```

## To resume an automatic scan

The following example continues from the `NextToken` the previous command
printed, and repeated calls walk the whole table.

```bash
aws dynamodb scan \
    --table-name <table> \
    --max-items 500 \
    --starting-token <next-token> \
    --query 'Items'
```

## To page through a query

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
