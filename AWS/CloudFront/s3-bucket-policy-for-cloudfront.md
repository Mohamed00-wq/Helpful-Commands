# S3 Bucket Policy for CloudFront

> S3 Bucket Policy for CloudFront. Part of the [CloudFront](../) cheatsheet.

## To set CloudFront bucket policy

The following example sets CloudFront bucket policy.

```bash
aws s3api put-bucket-policy --bucket my-bucket --policy file://cf-policy.json
```
