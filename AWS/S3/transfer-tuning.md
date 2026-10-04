# Transfer Tuning

> Transfer Tuning. Part of the [S3](../) cheatsheet.

## To tune transfer performance

The following example raises concurrency and lowers the multipart threshold, so
the CLI splits files into more, smaller parts and uploads them in parallel.

```bash
aws configure set default.s3.max_concurrent_requests 20
aws configure set default.s3.multipart_threshold 64MB
aws configure set default.s3.multipart_chunksize 16MB
```

## To check the current transfer settings

The following example reads back a configured S3 setting.

```bash
aws configure get default.s3.multipart_threshold
```
