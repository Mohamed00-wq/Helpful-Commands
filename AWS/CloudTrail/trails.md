# Trails

> Trails. Part of the [CloudTrail](../CloudTrail.md) cheatsheet.

## To create a multi-region trail

The following example creates a trail that collects management events from
every region in the account into a single S3 bucket.

```bash
aws cloudtrail create-trail \
    --name <trail> \
    --s3-bucket-name <bucket> \
    --s3-key-prefix <prefix> \
    --is-multi-region-trail \
    --include-global-service-events \
    --enable-log-file-validation
```

## To create a single-region trail

The following example creates a trail that only records calls made in the
region the trail lives in, which lowers storage cost when you do not need the
full account picture.

```bash
aws cloudtrail create-trail \
    --name <trail> \
    --s3-bucket-name <bucket> \
    --no-is-multi-region-trail \
    --enable-log-file-validation
```

## To start logging

The following example starts delivery, because a new trail does not record
anything until logging is started.

```bash
aws cloudtrail start-logging --name <trail>
```

## To stop logging

The following example pauses delivery without deleting the trail definition,
so you can restart it later with the same settings.

```bash
aws cloudtrail stop-logging --name <trail>
```

## To check whether a trail is delivering files

The following example lists the most recent log objects, because the trail
configuration does not report whether logging is currently running.

```bash
aws s3 ls "s3://<bucket>/<prefix>/" --recursive --region <region>
```

## To update a trail to a different bucket

The following example points an existing trail at a new bucket, which also
requires the new bucket to carry the CloudTrail bucket policy first.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --s3-bucket-name <bucket>
```

## To enable log file validation on an existing trail

The following example turns on digest files, which let you prove later that
nobody edited the log files in the bucket.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --enable-log-file-validation
```

## To get a trail ARN

The following example returns the ARN that the tagging calls and the bucket
policy both need.

```bash
aws cloudtrail describe-trails --trail-name-list <trail> \
    --query 'trailList[0].TrailARN' --output text
```

## To list trails

The following example returns the configuration of every trail in the region.

```bash
aws cloudtrail describe-trails \
    --query 'trailList[].[Name,IsMultiRegionTrail,HomeRegion,S3BucketName]'
```

## To list trails a page at a time

The following example returns one page of ten trail summaries, because
`list-trails` truncates the result set when you need to page through it
explicitly.

```bash
aws cloudtrail list-trails --max-items 10
```

## To continue from the previous page

The following example resumes from the token the previous page returned.

```bash
aws cloudtrail list-trails --starting-token <next-token>
```

## To tag a trail

The following example adds two tags, which are how you attribute a trail to a
team or an environment.

```bash
aws cloudtrail add-tags \
    --resource-id arn:aws:cloudtrail:<region>:<account-id>:trail/<trail> \
    --tags-list Key=Environment,Value=prod Key=Team,Value=platform
```

## To list the tags on a trail

The following example returns the tags on one or more trails.

```bash
aws cloudtrail list-tags \
    --resource-id-list arn:aws:cloudtrail:<region>:<account-id>:trail/<trail>
```

## To delete a trail

The following example removes the trail definition, which stops logging
immediately. The API has no `DryRun` parameter, so run `describe-trails` first
to confirm which trail you are about to drop, and note that the S3 objects
stay in the bucket after the trail is gone.

```bash
aws cloudtrail delete-trail --name <trail>
```
