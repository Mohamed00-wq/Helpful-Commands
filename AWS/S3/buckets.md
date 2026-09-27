# Buckets

> Buckets. Part of the [S3](../S3.md) cheatsheet.

## To create a bucket

The following example creates a bucket. Outside `us-east-1` you must supply a
location constraint, otherwise the call fails.

```bash
aws s3 mb s3://<bucket> --region <region> \
    --create-bucket-configuration LocationConstraint=<region>
```

## To list all buckets

## To list a bucket's contents

The following example lists the top level of a bucket.

```bash
aws s3 ls s3://<bucket>
```

## To list every object under a prefix

The following example lists all objects under a prefix, which is the form you
need before deleting or syncing a subtree.

```bash
aws s3 ls s3://<bucket>/<prefix>/ --recursive --human-readable
```

## To get the region of a bucket

The following example returns the bucket's region. An empty response means
`us-east-1`.

```bash
aws s3api get-bucket-location --bucket <bucket> --query 'LocationConstraint' --output text
```

## To list every bucket with its region

The following example loops over all buckets and prints the region of each one.

```bash
for bucket in $(aws s3api list-buckets --query 'Buckets[].Name' --output text); do
  region=$(aws s3api get-bucket-location --bucket "$bucket" --query 'LocationConstraint' --output text)
  echo "$bucket -> ${region:-us-east-1}"
done
```

## To delete an empty bucket

## To delete a bucket and everything in it

The following example deletes a non-empty bucket. Always run the `--dryrun`
version first to see exactly what would be removed.

```bash
aws s3 rb s3://<bucket> --force --dryrun
```

```bash
aws s3 rb s3://<bucket> --force
```
