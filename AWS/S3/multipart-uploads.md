# Multipart Uploads

> Multipart Uploads. Part of the [S3](../) cheatsheet.

## To start a multipart upload

The following example initiates a multipart upload and captures the upload ID
you need for every subsequent part call.

```bash
UPLOAD_ID=$(aws s3api create-multipart-upload --bucket <bucket> --key <key> \
    --query 'UploadId' --output text)
echo "Upload ID: $UPLOAD_ID"
```

## To list uploaded parts

The following example lists the parts uploaded so far.

```bash
aws s3api list-parts --bucket <bucket> --key <key> --upload-id "$UPLOAD_ID"
```

## To abort a multipart upload

The following example cancels an in-progress upload so you are not billed for
the parts.

```bash
aws s3api abort-multipart-upload --bucket <bucket> --key <key> --upload-id "$UPLOAD_ID"
```
