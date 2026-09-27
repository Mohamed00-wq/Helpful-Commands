# Objects

> Objects. Part of the [S3](../S3.md) cheatsheet.

## To upload a file

## To upload a folder

The following example uploads a directory recursively, which preserves the
local directory structure under the destination prefix.

```bash
aws s3 cp ./<folder> s3://<bucket>/<prefix> --recursive
```

## To download a file

The following example downloads a single object.

```bash
aws s3 cp s3://<bucket>/<key> ./
```

## To sync a local folder to a bucket

## To sync and delete removed files

The following example makes the destination match the source exactly by
deleting objects that no longer exist locally. Dry run it first.

```bash
aws s3 sync ./<folder> s3://<bucket>/<prefix> --delete --dryrun
```

## To copy between two buckets

The following example copies objects from one bucket to another.

```bash
aws s3 sync s3://<source-bucket> s3://<destination-bucket>
```

## To filter what gets synced

The following example syncs only images. Later `--include` rules win over
earlier ones, so exclude everything first.

```bash
aws s3 sync ./<folder> s3://<bucket> \
    --exclude "*" \
    --include "*.jpg" \
    --include "*.png"
```

## To sync to several buckets

The following example syncs the same folder to multiple buckets, skipping
temporary files.

```bash
for bucket in <bucket-1> <bucket-2> <bucket-3>; do
  aws s3 sync ./<folder> "s3://$bucket/<prefix>" --exclude "*.tmp"
done
```

## To delete a single object

The following example deletes one object.

```bash
aws s3 rm s3://<bucket>/<key>
```

## To delete every object under a prefix

The following example deletes a subtree. Dry run it first, because this cannot
be undone.

```bash
aws s3 rm s3://<bucket>/<prefix>/ --recursive --dryrun
```

## To delete objects matching a pattern

The following example deletes only log files, leaving every other object type
in place.

```bash
aws s3 rm s3://<bucket> --recursive --exclude "*" --include "*.log"
```

## To delete many objects in one API call

The following example deletes up to 1000 keys per request, which is far faster
than deleting them one at a time.

```bash
aws s3api delete-objects \
    --bucket <bucket> \
    --delete '{"Objects":[{"Key":"<key-1>"},{"Key":"<key-2>"}],"Quiet":true}'
```

## To generate a pre-signed URL

The following example creates a temporary download link that expires in one
hour.

```bash
aws s3 presign s3://<bucket>/<key> --expires-in 3600
```

## To find objects larger than a threshold

The following example lists keys and sizes for objects over roughly 1 MB.

```bash
aws s3api list-objects-v2 --bucket <bucket> \
    --query 'Contents[?Size>`1000000`].[Key,Size]' --output table
```
