# Encryption

> Encryption. Part of the [S3](../) cheatsheet.

## To check the default encryption

The following example reports the default server-side encryption for the
bucket, if any.

```bash
aws s3api get-bucket-encryption --bucket <bucket>
```

## To enable SSE-S3 encryption

The following example sets SSE-S3 as the default for all new objects.

```bash
aws s3api put-bucket-encryption \
    --bucket <bucket> \
    --server-side-encryption-configuration '{
      "Rules": [{
        "ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "AES256"}
      }]
    }'
```

## To enable SSE-KMS encryption

The following example sets SSE-KMS with a customer managed key, which also
gives you a key policy and an audit trail in CloudTrail.

```bash
aws s3api put-bucket-encryption \
    --bucket <bucket> \
    --server-side-encryption-configuration '{
      "Rules": [{
        "ApplyServerSideEncryptionByDefault": {
          "SSEAlgorithm": "aws:kms",
          "KMSMasterKeyID": "alias/<alias>"
        }
      }]
    }'
```

## To copy an object into another storage class

The following example rewrites an object so it is stored in
Standard-IA. Copying an object onto itself is how you change its storage class.

```bash
aws s3 cp s3://<bucket>/<key> s3://<bucket>/<key> --storage-class STANDARD_IA
```
