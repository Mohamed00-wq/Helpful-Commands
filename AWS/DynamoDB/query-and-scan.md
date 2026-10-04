# Query and Scan

> Query and Scan. Part of the [DynamoDB](../) cheatsheet.

## To query a partition

The following example reads every item in one partition, and a `query`
without a key condition is rejected because the partition key is mandatory.

```bash
aws dynamodb query \
    --table-name <table> \
    --key-condition-expression "<partition-key> = :pk" \
    --expression-attribute-values '{":pk":{"S":"<value>"}}'
```

## To query a range of sort keys

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

## To query the newest items first

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

## To query only a few attributes

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

## To scan a table

The following example reads every item in the table, and it will cost more the
larger the table is, so reach for `query` whenever a key condition will do.

```bash
aws dynamodb scan --table-name <table>
```

## To filter a scan

The following example applies the filter after the read, so you still pay for
every scanned item even though only some come back.

```bash
aws dynamodb scan \
    --table-name <table> \
    --filter-expression '#s = :status' \
    --expression-attribute-names '{"#s":"status"}' \
    --expression-attribute-values '{":status":{"S":"active"}}'
```

## To count items with a scan

The following example counts matching items without transferring any of them,
which is the cheapest way to size a table.

```bash
aws dynamodb scan \
    --table-name <table> \
    --filter-expression 'attribute_exists(#pk)' \
    --expression-attribute-names '{"#pk":"<partition-key>"}' \
    --select COUNT
```

## To scan in parallel

The following example splits the table into four segments, and running one
call per segment in parallel is how a full table read finishes in reasonable
time.

```bash
aws dynamodb scan \
    --table-name <table> \
    --segment 0 \
    --total-segments 4
```
