# Conditions and Locking

> Conditions and Locking. Part of the [DynamoDB](../DynamoDB.md) cheatsheet.

## To create an item only if it does not exist

The following example fails rather than overwriting, which is how you make
`put-item` safe to retry.

```bash
aws dynamodb put-item \
    --table-name <table> \
    --item file://item.json \
    --condition-expression 'attribute_not_exists(<partition-key>)'
```

## To guard an update with a version check

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

## To see why a condition failed

The following example returns the item as it looked when the condition check
failed, which saves a second `get-item` to find out why.

```bash
aws dynamodb delete-item \
    --table-name <table> \
    --key file://key.json \
    --condition-expression 'attribute_exists(<partition-key>)' \
    --return-values-on-condition-check-failure ALL_OLD
```

## To use an attribute whose name is a reserved word

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

## To return the item after a write

The following example returns the new or old item in the write's own response,
which saves a follow up read.

```bash
aws dynamodb put-item \
    --table-name <table> \
    --item file://item.json \
    --return-values ALL_NEW
```
