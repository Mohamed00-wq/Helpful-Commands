# Finding Changes

> Finding Changes. Part of the [CloudTrail](../CloudTrail.md) cheatsheet.

Event history covers the last 90 days and one region at a time, which is
enough for "who did this to me right now" questions. For anything older or
account wide, search the S3 log files described in [Log Files](log-files.md), or
run SQL over them in S3 with Athena.

## To find who changed an IAM role trust policy

The following example returns the calls that rewrote a role's trust policy,
parsed down to the time, the principal, and the source IP.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes \
        AttributeKey=EventName,AttributeValue=UpdateAssumeRolePolicy \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .userIdentity.arn, .sourceIPAddress] | @tsv'
```

## To find who deleted a bucket

The following example returns the call that removed a bucket, with the bucket
name and the caller's IP address.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=EventName,AttributeValue=DeleteBucket \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .requestParameters.bucketName, .sourceIPAddress] | @tsv'
```

## To find who changed a security group

The following example returns the ingress changes to a VPC security group.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes \
        AttributeKey=EventName,AttributeValue=AuthorizeSecurityGroupIngress \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .userIdentity.arn, .requestParameters] | @tsv'
```

## To find who signed in to the console

The following example returns console sign-ins, and the last field is the login
result, so `Failure` rows are the failed attempts worth looking at.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=EventName,AttributeValue=ConsoleLogin \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq -r '[.eventTime, .userIdentity.userName, .sourceIPAddress,
             .responseElements.ConsoleLogin] | @tsv'
```

## To read what a change actually contained

The following example returns the `requestParameters` block from an event, so
you can see the policy document or the old and new values.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes \
        AttributeKey=EventName,AttributeValue=PutUserPolicy \
    --query 'Events[].CloudTrailEvent' --output text \
  | jq '.requestParameters'
```
