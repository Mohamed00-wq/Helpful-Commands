# Event Source Mapping

> Event Source Mapping. Part of the [Lambda](../Lambda.md) cheatsheet.

## To list event source mappings

The following example lists event source mappings.

```bash
aws lambda list-event-source-mappings --function-name my-function
```

## To map SQS queue

The following example maps SQS queue.

```bash
aws lambda create-event-source-mapping
    --function-name my-function
    --event-source-arn arn:aws:sqs:us-east-1:ACCOUNT:my-queue
    --batch-size 10
```

## To map Kinesis stream

The following example maps Kinesis stream.

```bash
aws lambda create-event-source-mapping
    --function-name my-function
    --event-source-arn arn:aws:kinesis:us-east-1:ACCOUNT:stream/my-stream
    --starting-position LATEST
```

## To map DynamoDB stream

The following example maps DynamoDB stream.

```bash
aws lambda create-event-source-mapping
    --function-name my-function
    --event-source-arn arn:aws:dynamodb:us-east-1:ACCOUNT:table/my-table/stream/xxx
    --starting-position LATEST
```

## To update mapping config

The following example updates mapping config.

```bash
aws lambda update-event-source-mapping --uuid xxx --batch-size 50
```

## To delete event source mapping

The following example deletes event source mapping.

```bash
aws lambda delete-event-source-mapping --uuid xxx
```
