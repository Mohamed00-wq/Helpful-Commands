# SQS Messages

> SQS Messages. Part of the [SQS-SNS](../SQS-SNS.md) cheatsheet.

## To send a message

The following example sends a message.

```bash
aws sqs send-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --message-body "Hello"
```

## To send a FIFO message

The following example sends a FIFO message.

```bash
aws sqs send-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue.fifo
    --message-body "Hello"
    --message-group-id "group1"
```

## To receive messages (long polling)

The following example receives messages (long polling).

```bash
aws sqs receive-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --max-number-of-messages 10
    --wait-time-seconds 20
```

## To delete a processed message

The following example deletes a processed message.

```bash
aws sqs delete-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --receipt-handle "xxx"
```

## To extend message visibility

The following example extends message visibility.

```bash
aws sqs change-message-visibility
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --receipt-handle "xxx"
    --visibility-timeout 300
```

## To send batch messages

The following example sends batch messages.

```bash
aws sqs send-message-batch
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --entries file://batch.json
```

```bash
# Send message
aws sqs send-message \
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue \
    --message-body "Hello World"

# Receive (long polling)
aws sqs receive-message \
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue \
    --max-number-of-messages 10 \
    --wait-time-seconds 20
```
