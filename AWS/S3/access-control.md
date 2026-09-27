# Access Control

> Access Control. Part of the [S3](../S3.md) cheatsheet.

## To check the public access block

The following example reports the four account and bucket level public access
settings.

```bash
aws s3api get-public-access-block --bucket <bucket>
```

## To block all public access

The following example enables all four public access block settings, which is
the recommended baseline for a private bucket.

```bash
aws s3api put-public-access-block \
    --bucket <bucket> \
    --public-access-block-configuration \
    BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

## To apply a public access block to every bucket

The following example applies the same block to every bucket in the account.

```bash
for bucket in $(aws s3api list-buckets --query 'Buckets[].Name' --output text); do
  aws s3api put-public-access-block \
    --bucket "$bucket" \
    --public-access-block-configuration \
    BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
done
```

## To check the bucket policy

The following example returns the bucket policy as JSON.

```bash
aws s3api get-bucket-policy --bucket <bucket> --output text
```

## To attach a bucket policy

The following example attaches a public read policy. Combine it with a public
access block only if you intend the bucket to be public, because the two
settings conflict.

```bash
aws s3api put-bucket-policy --bucket <bucket> --policy file://policy.json
```

`policy.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicRead",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::<bucket>/*"
  }]
}
```

## To remove the bucket policy

The following example deletes the bucket policy entirely.

```bash
aws s3api delete-bucket-policy --bucket <bucket>
```
