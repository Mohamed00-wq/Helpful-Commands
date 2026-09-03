# 📨 SQS & SNS (Messaging & Notifications)

> Advanced SQS and SNS CLI commands for queues, topics, subscriptions, and dead-letter queues — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## SQS Queues

| Command | Description |
|---|---|
| `aws sqs list-queues` | List all queues |
| `aws sqs get-queue-url --queue-name my-queue` | Get queue URL |
| `aws sqs create-queue --queue-name my-queue` | Create a standard queue |
| `aws sqs create-queue --queue-name my-queue --attributes '{"FifoQueue":"true","ContentBasedDeduplication":"true"}'` | Create a FIFO queue |
| `aws sqs delete-queue --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue` | Delete a queue |
| `aws sqs purge-queue --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue` | Purge all messages |
| `aws sqs get-queue-attributes --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --attribute-names All` | Get queue attributes |

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

---

## SQS Messages

| Command | Description |
|---|---|
| `aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --message-body "Hello"` | Send a message |
| `aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue.fifo --message-body "Hello" --message-group-id "group1"` | Send a FIFO message |
| `aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --max-number-of-messages 10 --wait-time-seconds 20` | Receive messages (long polling) |
| `aws sqs delete-message --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --receipt-handle "xxx"` | Delete a processed message |
| `aws sqs change-message-visibility --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --receipt-handle "xxx" --visibility-timeout 300` | Extend message visibility |
| `aws sqs send-message-batch --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --entries file://batch.json` | Send batch messages |

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

---

## SQS Dead-Letter Queue

| Command | Description |
|---|---|
| `aws sqs set-queue-attributes --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue --attributes '{"RedrivePolicy":"{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:123456789012:my-dlq\",\"maxReceiveCount\":\"3\"}"}'` | Set DLQ policy |
| `aws sqs get-dead-letter-queue-url --queue-arn arn:aws:sqs:us-east-1:123456789012:my-queue` | Get DLQ URL |

```bash
aws sqs set-queue-attributes \
  --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue \
  --attributes '{"RedrivePolicy":"{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:123456789012:my-dlq\",\"maxReceiveCount\":\"3\"}"}'
```

---

## SNS Topics

| Command | Description |
|---|---|
| `aws sns list-topics` | List all topics |
| `aws sns create-topic --name my-topic` | Create a standard topic |
| `aws sns create-topic --name my-topic.fifo --attributes FifoTopic=true` | Create a FIFO topic |
| `aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic` | Delete a topic |
| `aws sns get-topic-attributes --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic` | Get topic attributes |

```bash
# Create standard topic
aws sns create-topic --name my-topic

# Create FIFO topic
aws sns create-topic --name my-topic.fifo --attributes FifoTopic=true
```

---

## SNS Subscriptions

| Command | Description |
|---|---|
| `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol email --notification-endpoint user@example.com` | Subscribe by email |
| `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol sms --notification-endpoint +1234567890` | Subscribe by SMS |
| `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol lambda --notification-endpoint arn:aws:lambda:us-east-1:123456789012:function:my-function` | Subscribe Lambda |
| `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol sqs --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue` | Subscribe SQS |
| `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol https --notification-endpoint https://example.com/webhook` | Subscribe HTTPS |
| `aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic` | List subscriptions |
| `aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:123456789012:my-topic:xxx` | Unsubscribe |
| `aws sns confirm-subscription --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --token xxx` | Confirm a subscription |

```bash
# Subscribe email
aws sns subscribe \
  --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic \
  --protocol email \
  --notification-endpoint user@example.com

# Subscribe Lambda
aws sns subscribe \
  --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic \
  --protocol lambda \
  --notification-endpoint arn:aws:lambda:us-east-1:123456789012:function:my-function
```

---

## SNS Publishing

| Command | Description |
|---|---|
| `aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --message "Hello"` | Publish a message |
| `aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --message "Alert" --subject "High CPU"` | Publish with subject |
| `aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --message-structure json --message '{"default":"Default message","email":"Email version","sms":"SMS version"}'` | Publish with protocol-specific messages |
| `aws sns publish --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic.fifo --message "Order 123" --message-group-id "orders" --message-deduplication-id "dedup-001"` | Publish FIFO message |

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

---

## SNS Message Filtering

| Command | Description |
|---|---|
| `aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --protocol sqs --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue --attributes '{"FilterPolicy":"{\"store\":[\"example_corp\"]}"}'` | Subscribe with filter |
| `aws sns set-subscription-attributes --subscription-arn arn:... --attribute-name FilterPolicy --attribute-value '{"store":["example_corp"]}'` | Update filter policy |

```bash
# Subscribe with filter
aws sns subscribe \
  --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic \
  --protocol sqs \
  --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue \
  --attributes '{"FilterPolicy":"{\"store\":[\"example_corp\"]}"}'
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
