# CloudTrail (API Activity Audit)

> Commands for trails, event selectors, insight selectors, CloudTrail Lake,
> event history lookup, and reading the gzipped log files in S3.

CloudTrail records the control plane: every AWS API call made through the
console, CLI, or SDK, including who made it and from which IP address. It does
not record data plane activity by default. S3 object access, Lambda
invocations, and DynamoDB item-level reads and writes appear in a trail only
after you turn on data events with an event data selector, and those data
events are billed separately from management events.

A trail also needs write access to its bucket before it can be created, and
the Workflows section at the end of this file starts with that bucket policy.

## Trails

### To create a multi-region trail

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

### To create a single-region trail

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

### To start logging

The following example starts delivery, because a new trail does not record
anything until logging is started.

```bash
aws cloudtrail start-logging --name <trail>
```

### To stop logging

The following example pauses delivery without deleting the trail definition,
so you can restart it later with the same settings.

```bash
aws cloudtrail stop-logging --name <trail>
```

### To check whether a trail is delivering files

The following example lists the most recent log objects, because the trail
configuration does not report whether logging is currently running.

```bash
aws s3 ls "s3://<bucket>/<prefix>/" --recursive --region <region>
```

### To update a trail to a different bucket

The following example points an existing trail at a new bucket, which also
requires the new bucket to carry the CloudTrail bucket policy first.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --s3-bucket-name <bucket>
```

### To enable log file validation on an existing trail

The following example turns on digest files, which let you prove later that
nobody edited the log files in the bucket.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --enable-log-file-validation
```

### To get a trail ARN

The following example returns the ARN that the tagging calls and the bucket
policy both need.

```bash
aws cloudtrail describe-trails --trail-name-list <trail> \
    --query 'trailList[0].TrailARN' --output text
```

### To list trails

The following example returns the configuration of every trail in the region.

```bash
aws cloudtrail describe-trails \
    --query 'trailList[].[Name,IsMultiRegionTrail,HomeRegion,S3BucketName]'
```

### To list trails a page at a time

The following example returns one page of ten trail summaries, because
`list-trails` truncates the result set when you need to page through it
explicitly.

```bash
aws cloudtrail list-trails --max-items 10
```

### To continue from the previous page

The following example resumes from the token the previous page returned.

```bash
aws cloudtrail list-trails --starting-token <next-token>
```

### To tag a trail

The following example adds two tags, which are how you attribute a trail to a
team or an environment.

```bash
aws cloudtrail add-tags \
    --resource-id arn:aws:cloudtrail:<region>:<account-id>:trail/<trail> \
    --tags-list Key=Environment,Value=prod Key=Team,Value=platform
```

### To list the tags on a trail

The following example returns the tags on one or more trails.

```bash
aws cloudtrail list-tags \
    --resource-id-list arn:aws:cloudtrail:<region>:<account-id>:trail/<trail>
```

### To delete a trail

The following example removes the trail definition, which stops logging
immediately. The API has no `DryRun` parameter, so run `describe-trails` first
to confirm which trail you are about to drop, and note that the S3 objects
stay in the bucket after the trail is gone.

```bash
aws cloudtrail delete-trail --name <trail>
```

## Event Data Selectors

A trail records management events by default. Data events, which are the reads
and writes on the objects themselves, are opt-in per resource type and cost
extra.

### To record S3 object access

The following example records every S3 object access on the whole bucket.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --event-selectors file://s3-data-events.json
```

`s3-data-events.json`:

```json
[
  {
    "ReadWriteType": "All",
    "IncludeManagementEvents": true,
    "DataResources": [
      {
        "Type": "AWS::S3Object",
        "Values": ["arn:aws:s3:::<bucket>/"]
      }
    ]
  }
]
```

### To record Lambda invocations

The following example records invocations of a single function, which is the
narrowest selector you can use.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --event-selectors file://lambda-data-events.json
```

`lambda-data-events.json`:

```json
[
  {
    "ReadWriteType": "All",
    "IncludeManagementEvents": true,
    "DataResources": [
      {
        "Type": "AWS::LambdaFunction",
        "Values": [
          "arn:aws:lambda:<region>:<account-id>:function:<function>"
        ]
      }
    ]
  }
]
```

### To record DynamoDB item access

The following example records item-level reads and writes on a table, which is
the only way to see who read a specific item.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --event-selectors file://dynamodb-data-events.json
```

`dynamodb-data-events.json`:

```json
[
  {
    "ReadWriteType": "All",
    "IncludeManagementEvents": true,
    "DataResources": [
      {
        "Type": "AWS::DynamoDBTable",
        "Values": [
          "arn:aws:dynamodb:<region>:<account-id>:table/<table>"
        ]
      }
    ]
  }
]
```

### To record S3 access under one prefix only

The following example limits the S3 data events to a prefix, so unrelated
objects in the same bucket do not add to the bill.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --event-selectors file://s3-prefix-data-events.json
```

`s3-prefix-data-events.json`:

```json
[
  {
    "ReadWriteType": "All",
    "IncludeManagementEvents": true,
    "DataResources": [
      {
        "Type": "AWS::S3Object",
        "Values": ["arn:aws:s3:::<bucket>/<prefix>/"]
      }
    ]
  }
]
```

### To read the current selectors

The following example returns the selectors on a trail, which is the fastest
way to check whether data events are on.

```bash
aws cloudtrail get-event-selectors --trail-name <trail>
```

### To stop recording data events

The following example replaces the selectors with a management-events-only
set, which is how you turn data events back off.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --event-selectors '[{"ReadWriteType":"All","IncludeManagementEvents":true}]'
```

### To filter events with an advanced selector

The following example uses an advanced selector to record only write calls
against a bucket, which basic selectors cannot express.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --advanced-event-selectors file://advanced-selectors.json
```

`advanced-selectors.json`:

```json
[
  {
    "Name": "s3-writes-only",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::S3Object"] },
      { "Field": "readOnly", "Equals": ["false"] }
    ]
  }
]
```

Advanced selectors replace basic selectors on a trail. The two forms cannot be
mixed in one call.

## Insight Selectors

Insights are CloudTrail's own anomaly detection. They write summary events into
the trail and event history so you see the spike without reading every record.

### To turn on call rate and error rate insights

The following example enables both insight types, which is what surfaces
"An API call rate is spiking" notifications.

```bash
aws cloudtrail put-insight-selectors \
    --trail-name <trail> \
    --insight-selectors file://insight-selectors.json
```

`insight-selectors.json`:

```json
[
  { "InsightType": "ApiCallRateInsight" },
  { "InsightType": "ApiErrorRateInsight" }
]
```

### To check which insights are on

The following example returns the insight selectors for a trail.

```bash
aws cloudtrail get-insight-selectors --trail-name <trail>
```

## CloudWatch Logs

A trail can send the same events to a log group as well as to S3, which lets
`filter-log-events` search them without touching the bucket.

### To deliver logs to a log group

The following example adds CloudWatch Logs delivery to a trail, where the role
needs `logs:CreateLogStream` and `logs:PutLogEvents` on the log group.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --cloud-watch-logs-log-group-arn \
        arn:aws:logs:<region>:<account-id>:log-group:<log-group>:* \
    --cloud-watch-logs-role-arn arn:aws:iam::<account-id>:role/<role>
```

### To stop the CloudWatch Logs delivery

The following example removes the log group and role from the trail, which
leaves S3 delivery untouched.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --cloud-watch-logs-log-group-arn "" \
    --cloud-watch-logs-role-arn ""
```

### To search the delivered events

The following example filters the trail's events in the log group, which is the
fast path for a single investigation.

```bash
aws logs filter-log-events \
    --log-group-name <log-group> \
    --filter-pattern '{ $.eventName = "DeleteTrail" }' \
    --start-time 1790294400000
```

## CloudTrail Lake

An event data store is the queryable, managed copy of a trail. It costs per
day of retention rather than per gigabyte stored, and the console runs SQL
against it.

### To create an event data store

The following example creates a data store that keeps seven days of
multi-region events.

```bash
aws cloudtrail create-event-data-store \
    --name <data-store> \
    --retention-period 7 \
    --multi-region-enabled \
    --termination-protection-enabled \
    --tags-list Key=Environment,Value=prod
```

### To check the status of a data store

The following example returns the data store's ARN and ingestion status,
because a store takes a few minutes to start ingesting.

```bash
aws cloudtrail get-event-data-store \
    --event-data-store arn:aws:cloudtrail:<region>:<account-id>:eventdatastore/<data-store>
```

### To list data stores

The following example returns every data store in the account or organization.

```bash
aws cloudtrail list-event-data-stores --max-results 10
```

### To send a partner's events into a data store

The following example creates a channel, which is how a partner integration
feeds CloudTrail Lake.

```bash
aws cloudtrail create-channel \
    --name <channel> \
    --source "aws-service/<service>" \
    --destinations file://channel-destinations.json
```

`channel-destinations.json`:

```json
[
  {
    "Type": "EVENT_DATA_STORE",
    "Location": "arn:aws:cloudtrail:<region>:<account-id>:eventdatastore/<data-store>"
  }
]
```

### To check a channel's ingestion status

The following example returns where a channel's events are going and whether
they are arriving.

```bash
aws cloudtrail get-channel --channel <channel>
```

### To delete a data store

The following example removes a data store and everything retained in it, so
unlock termination protection first or the call is rejected.

```bash
aws cloudtrail delete-event-data-store \
    --event-data-store arn:aws:cloudtrail:<region>:<account-id>:eventdatastore/<data-store>
```

## Shadow Trails

Shadow trails are copies of a member account's trail created automatically for
a partner integration, such as a SIEM. The AWS CLI has no call to create one;
you list and read them like any other trail.

### To list shadow trails

The following example includes shadow trails, which `describe-trails` leaves
out by default.

```bash
aws cloudtrail describe-trails --include-shadow-trails
```

### To read a shadow trail's configuration

The following example returns one shadow trail by name, and its ARN is the
resource ID that `add-tags` and `list-tags` expect.

```bash
aws cloudtrail describe-trails \
    --trail-name-list <trail> \
    --include-shadow-trails
```

### To read a shadow trail's selectors

The following example returns the data events a partner is collecting, which
is the usual reason to look at a shadow trail.

```bash
aws cloudtrail get-event-selectors --trail-name <trail>
```

## Organization Trails

An organization trail is created in the management account or in a delegated
administrator account and collects events from every member account.

### To create an organization trail

The following example creates an organization trail in the management account,
which is the only account that can create one.

```bash
aws cloudtrail create-trail \
    --name <trail> \
    --s3-bucket-name <bucket> \
    --is-organization-trail \
    --is-multi-region-trail
```

### To create an organization trail in a delegated admin

The following example creates the same trail from the delegated administrator
account, which is what you use when security owns a separate member account.

```bash
aws cloudtrail create-trail \
    --name <trail> \
    --s3-bucket-name <bucket> \
    --is-organization-trail \
    --is-multi-region-trail \
    --profile <profile>
```

### To read the organization trail configuration

The following example returns the trail as the delegated administrator sees
it, including the flag that marks it as organization wide.

```bash
aws cloudtrail describe-trails \
    --query 'trailList[].[Name,IsOrganizationTrail,IsMultiRegionTrail]' \
    --profile <profile>
```

### To tag an organization trail

The following example tags the trail once for the whole organization, because
member accounts cannot change the shared trail.

```bash
aws cloudtrail add-tags \
    --resource-id arn:aws:cloudtrail:<region>:<account-id>:trail/<trail> \
    --tags-list Key=Environment,Value=prod
```

## Event History

Event history is a searchable copy of management events kept in the region for
the configured retention window, 90 days by default. It never contains data
events, and it is what `lookup-events` reads.

### To find recent management events

The following example returns the last few hours of management events in the
current region.

```bash
aws cloudtrail lookup-events \
    --start-time "2026-09-26T00:00:00Z" \
    --end-time "2026-09-26T12:00:00Z" \
    --max-items 50
```

### To find one API call by name

The following example returns every `DeleteTrail` call, and you may only set
one attribute key per lookup.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=EventName,AttributeValue=DeleteTrail \
    --start-time "2026-09-26T00:00:00Z"
```

### To find calls made by a user

The following example returns every call made by one IAM user.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=Username,AttributeValue=<username> \
    --start-time "2026-09-26T00:00:00Z"
```

### To find insight and digest events

The following example returns the anomaly and digest events rather than the
raw API calls, which is a cheap way to see what CloudTrail flagged.

```bash
aws cloudtrail lookup-events --event-category insight --max-items 10
```

### To page through event history

The following example returns the next page, because `lookup-events` returns
at most 50 events per call and stops there.

```bash
aws cloudtrail lookup-events \
    --start-time "2026-09-26T00:00:00Z" \
    --starting-token <next-token>
```

## Finding Changes

Event history covers the last 90 days and one region at a time, which is
enough for "who did this to me right now" questions. For anything older or
account wide, search the S3 log files from the section below, or run SQL over
them in S3 with Athena.

### To find who changed an IAM role trust policy

The following example returns the calls that rewrote a role's trust policy,
parsed down to the time, the principal, and the source IP.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes \
        AttributeKey=EventName,AttributeValue=UpdateAssumeRolePolicy \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .userIdentity.arn, .sourceIPAddress] | @tsv'
```

### To find who deleted a bucket

The following example returns the call that removed a bucket, with the bucket
name and the caller's IP address.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=EventName,AttributeValue=DeleteBucket \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .requestParameters.bucketName, .sourceIPAddress] | @tsv'
```

### To find who changed a security group

The following example returns the ingress changes to a VPC security group.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes \
        AttributeKey=EventName,AttributeValue=AuthorizeSecurityGroupIngress \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .userIdentity.arn, .requestParameters] | @tsv'
```

### To find who signed in to the console

The following example returns console sign-ins, and the last field is the login
result, so `Failure` rows are the failed attempts worth looking at.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=EventName,AttributeValue=ConsoleLogin \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .userIdentity.userName, .sourceIPAddress,
             .responseElements.ConsoleLogin] | @tsv'
```

### To read what a change actually contained

The following example returns the `requestParameters` block from an event, so
you can see the policy document or the old and new values.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes \
        AttributeKey=EventName,AttributeValue=PutUserPolicy \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq '.requestParameters'
```

## Log Files

CloudTrail writes gzipped JSON Lines to S3, so every file holds many records,
one JSON object per line. `jq` reads that stream directly, and the files
arrive about fifteen minutes behind the events. When log file validation is
on, a `CloudTrail-Digest` folder sits alongside the dated folders and holds
the checksums.

### To list the log objects for one day

The following example lists the objects for a single day, because the
`AWSLogs` prefix is partitioned by account, region, and date.

```bash
aws s3 ls "s3://<bucket>/AWSLogs/<account-id>/CloudTrail/<region>/2026/09/26/" \
    --recursive --region <region>
```

### To download the log files

The following example fetches the whole trail prefix, which also pulls the
`CloudTrail-Digest` folder you need for the tamper check below.

```bash
aws s3 sync \
    "s3://<bucket>/AWSLogs/<account-id>/CloudTrail/<region>/" \
    ./cloudtrail-logs/ --region <region>
```

### To read the records in a log file

The following example prints one line per record without printing the binary
body, which keeps the output readable.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -c '{time: .eventTime, name: .eventName, who: .userIdentity.arn}'
```

### To count the calls made by each principal

The following example tallies calls per user or assumed role, which is how you
find the noisiest principal in an account.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -r '.userIdentity.arn // "unknown"' \
  | sort | uniq -c | sort -rn | head
```

### To list the failed calls

The following example counts errors by code, which is how a misconfigured
automation shows up in a trail.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -r 'select(.errorCode != null) | .errorCode' \
  | sort | uniq -c | sort -rn
```

### To find calls from one IP address

The following example returns every call made from a single address, which
covers failed sign-ins and brute force attempts.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -c 'select(.sourceIPAddress == "<ip-address>")
          | {time: .eventTime, name: .eventName, error: .errorCode}'
```

### To check whether a file was tampered with

The following example verifies the gzipped files against the digest files
CloudTrail writes next to them, which is only possible when log file
validation is enabled.

```bash
zcat ./cloudtrail-logs/CloudTrail-Digest/*.json.gz \
  | jq -r '.files[] | select(.filePath | endswith(".json.gz"))
          | "\(.hashHex)  \(.filePath)"' \
  | sha256sum --check --ignore-missing
```

Run it from the directory the digest's `filePath` values are relative to, and
every listed file must report `OK`.

## Workflows

### To build a working trail from scratch

The following steps create a bucket, grant CloudTrail access to it, create the
trail, add data events, start delivery, and confirm files are landing. Run them
in order, because each one fails without the step before it.

1. Create the log bucket. The bucket name is globally unique, so you may have
   to keep adding a suffix until you find a free name.

   ```bash
   aws s3api create-bucket --bucket <bucket> --region <region>
   aws s3api put-bucket-versioning \
       --bucket <bucket> --versioning-configuration Status=Enabled
   ```

2. Attach the bucket policy. Without the `GetBucketAcl` and `PutObject`
   statements, `create-trail` fails with `InsufficientS3BucketPolicy`.

   ```bash
   aws s3api put-bucket-policy --bucket <bucket> \
       --policy file://cloudtrail-bucket-policy.json
   ```

   `cloudtrail-bucket-policy.json`:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "AWSCloudTrailAclCheck",
         "Effect": "Allow",
         "Principal": { "Service": "cloudtrail.amazonaws.com" },
         "Action": "s3:GetBucketAcl",
         "Resource": "arn:aws:s3:::<bucket>"
       },
       {
         "Sid": "AWSCloudTrailWrite",
         "Effect": "Allow",
         "Principal": { "Service": "cloudtrail.amazonaws.com" },
         "Action": "s3:PutObject",
         "Resource": "arn:aws:s3:::<bucket>/AWSLogs/<account-id>/*"
       }
     ]
   }
   ```

   A bucket encrypted with a KMS key needs the same service principal in the
   key policy, or `create-trail` fails with
   `InsufficientEncryptionPolicy`.

3. Create the trail and capture its ARN, which is the identifier the
   `add-tags` and `list-tags` calls take.

   ```bash
   TRAIL_ARN=$(aws cloudtrail create-trail \
       --name <trail> \
       --s3-bucket-name <bucket> \
       --is-multi-region-trail \
       --include-global-service-events \
       --enable-log-file-validation \
       --query 'TrailARN' --output text)
   echo "$TRAIL_ARN"
   ```

4. Add the data events you need. This step is the one most often skipped, and
   without it object reads never appear anywhere.

   ```bash
   aws cloudtrail put-event-selectors \
       --trail-name <trail> \
       --event-selectors file://s3-data-events.json
   ```

5. Start logging. A created trail is not yet recording.

   ```bash
   aws cloudtrail start-logging --name <trail>
   ```

6. Confirm delivery. Wait roughly fifteen minutes, then look for today's
   objects under the `AWSLogs` prefix.

   ```bash
   aws s3 ls "s3://<bucket>/AWSLogs/<account-id>/" --recursive \
       --region <region>
   ```

### To tear down a trail

The following order stops delivery before removing the definition, and leaves
the log objects in place so the audit history survives.

```bash
# Confirm what you are about to delete
aws cloudtrail describe-trails --trail-name-list <trail>

# Stop delivery, then remove the trail
aws cloudtrail stop-logging --name <trail>
aws cloudtrail delete-trail --name <trail>

# The log files are still in the bucket; retire them with a lifecycle rule
aws s3api put-bucket-lifecycle-configuration \
    --bucket <bucket> \
    --lifecycle-configuration file://lifecycle.json
```
