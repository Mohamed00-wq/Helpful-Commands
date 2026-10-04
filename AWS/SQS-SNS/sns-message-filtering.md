# SNS Message Filtering

> SNS Message Filtering. Part of the [SQS-SNS](../) cheatsheet.

## To subscribe with filter

The following example subscribes with filter.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol sqs
    --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue
    --attributes '{"FilterPolicy":"{\"store\":[\"example_corp\"]}"}'
```

## To update filter policy

The following example updates filter policy.

```bash
aws sns set-subscription-attributes
    --subscription-arn arn:...
    --attribute-name FilterPolicy
    --attribute-value '{"store":["example_corp"]}'
```
