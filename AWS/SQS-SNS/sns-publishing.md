# SNS Publishing

> SNS Publishing. Part of the [SQS-SNS](../) cheatsheet.

## To publish a message

The following example publishes a message.

```bash
aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --message "Hello"
```

## To publish with subject

The following example publishes with subject.

```bash
aws sns publish
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --message "Alert"
    --subject "High CPU"
```

## To publish with protocol-specific messages

The following example publishes with protocol-specific messages.

```bash
aws sns publish
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --message-structure json
    --message '{"default":"Default message","email":"Email version","sms":"SMS version"}'
```

## To publish FIFO message

The following example publishes FIFO message.

```bash
aws sns publish
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic.fifo
    --message "Order 123"
    --message-group-id "orders"
    --message-deduplication-id "dedup-001"
```

```bash
# Basic publish
aws sns publish \
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic \
    --message "Hello World"

# FIFO publish
aws sns publish \
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic.fifo \
    --message "Order 123" \
    --message-group-id "orders" \
    --message-deduplication-id "dedup-001"
```
