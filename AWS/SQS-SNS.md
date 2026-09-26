# SQS and SNS (Messaging and Notifications)

> Commands for queues, topics, subscriptions, FIFO ordering, and dead letter queues.

## SQS Queues

### To list all queues

The following example lists all queues.

```bash
aws sqs list-queues
```

### To get queue URL

The following example gets queue URL.

```bash
aws sqs get-queue-url --queue-name my-queue
```

### To create a standard queue

The following example creates a standard queue.

```bash
aws sqs create-queue --queue-name my-queue
```

### To create a FIFO queue

The following example creates a FIFO queue.

```bash
aws sqs create-queue
    --queue-name my-queue
    --attributes '{"FifoQueue":"true","ContentBasedDeduplication":"true"}'
```

### To delete a queue

The following example deletes a queue.

```bash
aws sqs delete-queue --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
```

### To purge all messages

The following example purges all messages.

```bash
aws sqs purge-queue --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
```

### To get queue attributes

The following example gets queue attributes.

```bash
aws sqs get-queue-attributes
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --attribute-names All
```

```bash
# Create standard queue
aws sqs create-queue --queue-name my-queue

# Create FIFO queue
aws sqs create-queue \
    --queue-name my-queue.fifo \
    --attributes '{"FifoQueue":"true","ContentBasedDeduplication":"true"}'

# Delete
aws sqs delete-queue \
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
```

## SQS Messages

### To send a message

The following example sends a message.

```bash
aws sqs send-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --message-body "Hello"
```

### To send a FIFO message

The following example sends a FIFO message.

```bash
aws sqs send-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue.fifo
    --message-body "Hello"
    --message-group-id "group1"
```

### To receive messages (long polling)

The following example receives messages (long polling).

```bash
aws sqs receive-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --max-number-of-messages 10
    --wait-time-seconds 20
```

### To delete a processed message

The following example deletes a processed message.

```bash
aws sqs delete-message
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --receipt-handle "xxx"
```

### To extend message visibility

The following example extends message visibility.

```bash
aws sqs change-message-visibility
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --receipt-handle "xxx"
    --visibility-timeout 300
```

### To send batch messages

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

## SQS Dead-Letter Queue

### To set DLQ policy

The following example sets DLQ policy.

```bash
aws sqs set-queue-attributes
    --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
    --attributes '{"RedrivePolicy":"{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:123456789012:my-dlq\",\"maxReceiveCount\":\"3\"}"}'
```

### To get DLQ URL

The following example gets DLQ URL.

```bash
aws sqs get-dead-letter-queue-url --queue-arn arn:aws:sqs:us-east-1:123456789012:my-queue
```

## SNS Topics

### To list all topics

The following example lists all topics.

```bash
aws sns list-topics
```

### To create a standard topic

The following example creates a standard topic.

```bash
aws sns create-topic --name my-topic
```

### To create a FIFO topic

The following example creates a FIFO topic.

```bash
aws sns create-topic --name my-topic.fifo --attributes FifoTopic=true
```

### To delete a topic

The following example deletes a topic.

```bash
aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
```

### To get topic attributes

The following example gets topic attributes.

```bash
aws sns get-topic-attributes --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
```

## SNS Subscriptions

### To subscribe by email

The following example subscribes by email.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol email
    --notification-endpoint user@example.com
```

### To subscribe by SMS

The following example subscribes by SMS.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol sms
    --notification-endpoint +1234567890
```

### To subscribe Lambda

The following example subscribes Lambda.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol lambda
    --notification-endpoint arn:aws:lambda:us-east-1:123456789012:function:my-function
```

### To subscribe SQS

The following example subscribes SQS.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol sqs
    --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue
```

### To subscribe HTTPS

The following example subscribes HTTPS.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol https
    --notification-endpoint https://example.com/webhook
```

### To list subscriptions

The following example lists subscriptions.

```bash
aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
```

### To unsubscribe

The following example unsubscribes .

```bash
aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:123456789012:my-topic:xxx
```

### To confirm a subscription

The following example confirms a subscription.

```bash
aws sns confirm-subscription --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --token xxx
```

## SNS Publishing

### To publish a message

The following example publishes a message.

```bash
aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --message "Hello"
```

### To publish with subject

The following example publishes with subject.

```bash
aws sns publish
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --message "Alert"
    --subject "High CPU"
```

### To publish with protocol-specific messages

The following example publishes with protocol-specific messages.

```bash
aws sns publish
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --message-structure json
    --message '{"default":"Default message","email":"Email version","sms":"SMS version"}'
```

### To publish FIFO message

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

## SNS Message Filtering

### To subscribe with filter

The following example subscribes with filter.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol sqs
    --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue
    --attributes '{"FilterPolicy":"{\"store\":[\"example_corp\"]}"}'
```

### To update filter policy

The following example updates filter policy.

```bash
aws sns set-subscription-attributes
    --subscription-arn arn:...
    --attribute-name FilterPolicy
    --attribute-value '{"store":["example_corp"]}'
```
