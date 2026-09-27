# Event Data Selectors

> Event Data Selectors. Part of the [CloudTrail](../CloudTrail.md) cheatsheet.

A trail records management events by default. Data events, which are the reads
and writes on the objects themselves, are opt-in per resource type and cost
extra.

## To record S3 object access

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

## To record Lambda invocations

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

## To record DynamoDB item access

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

## To record S3 access under one prefix only

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

## To read the current selectors

The following example returns the selectors on a trail, which is the fastest
way to check whether data events are on.

```bash
aws cloudtrail get-event-selectors --trail-name <trail>
```

## To stop recording data events

The following example replaces the selectors with a management-events-only
set, which is how you turn data events back off.

```bash
aws cloudtrail put-event-selectors \
    --trail-name <trail> \
    --event-selectors '[{"ReadWriteType":"All","IncludeManagementEvents":true}]'
```

## To filter events with an advanced selector

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
