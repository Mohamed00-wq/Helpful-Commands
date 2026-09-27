# SQS Dead-Letter Queue

> SQS Dead-Letter Queue. Part of the [SQS-SNS](../SQS-SNS.md) cheatsheet.

## To set DLQ policy

The following example sets DLQ policy.

```bash
aws sqs set-queue-attributes
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --attributes '{"RedrivePolicy":"{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:123456789012:my-dlq\",\"maxReceiveCount\":\"3\"}"}'
```

## To get DLQ URL

The following example gets DLQ URL.

```bash
aws sqs get-dead-letter-queue-url --queue-arn arn:aws:sqs:us-east-1:123456789012:my-queue
```
