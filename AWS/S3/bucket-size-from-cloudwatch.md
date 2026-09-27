# Bucket Size From CloudWatch

> Bucket Size From CloudWatch. Part of the [S3](../S3.md) cheatsheet.

## To get the bucket size from CloudWatch

The following example reads the daily `BucketSizeBytes` statistic, which is
stored in CloudWatch and is more accurate than summing `s3 ls` output.

```bash
aws cloudwatch get-metric-statistics \
    --namespace AWS/S3 \
    --metric-name BucketSizeBytes \
    --dimensions Name=BucketName,Value=<bucket> Name=StorageType,Value=StandardStorage \
    --start-time "$(date -u -d '2 days ago' +%Y-%m-%dT%H:%M:%S)" \
    --end-time "$(date -u +%Y-%m-%dT%H:%M:%S)" \
    --period 86400 --statistics Average
```
