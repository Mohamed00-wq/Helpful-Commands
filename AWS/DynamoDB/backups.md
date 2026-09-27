# Backups

> Backups. Part of the [DynamoDB](../DynamoDB.md) cheatsheet.

## To take a backup before deleting

The following example captures an on-demand backup, which is the only way
back after `delete-table`.

```bash
aws dynamodb create-backup \
    --table-name <table> \
    --backup-name <backup>
```

## To list a table's backups

The following example returns the backups you can restore from, with their
status and expiry.

```bash
aws dynamodb list-backups --table-name <table>
```

## To export a table to S3

The following example exports a point in time snapshot as DynamoDB JSON,
which is the format to use for a long term copy in a bucket.

```bash
aws dynamodb export-table-to-point-in-time \
    --table-arn arn:aws:dynamodb:<region>:<account-id>:table/<table> \
    --s3-bucket <bucket> \
    --export-format DYNAMODB_JSON
```
