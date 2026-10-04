# Storage Classes

> Storage Classes. Part of the [S3](../) cheatsheet.

## To upload to a specific storage class

The following examples upload an object to each of the archival storage
classes. Glacier and Deep Archive objects are not readable for hours after
upload unless you restore them.

```bash
aws s3 cp <file> s3://<bucket>/ --storage-class STANDARD_IA
aws s3 cp <file> s3://<bucket>/ --storage-class GLACIER
aws s3 cp <file> s3://<bucket>/ --storage-class DEEP_ARCHIVE
```

## To restore a Glacier object

The following example starts a restore. Standard retrieval takes several
hours; Expedited takes minutes and costs more.

```bash
aws s3api restore-object \
    --bucket <bucket> \
    --key <key> \
    --restore-request '{"Days":1,"GlacierJobParameters":{"Tier":"Standard"}}'
```

## To check a restore job

The following example polls the restore request until it reports completion.

```bash
aws s3api head-object --bucket <bucket> --key <key>
```
