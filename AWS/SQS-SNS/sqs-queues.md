# SQS Queues

> SQS Queues. Part of the [SQS-SNS](../) cheatsheet.

## To list all queues

The following example lists all queues.

```bash
aws sqs list-queues
```

## To get queue URL

The following example gets queue URL.

```bash
aws sqs get-queue-url --queue-name my-queue
```

## To create a standard queue

The following example creates a standard queue.

```bash
aws sqs create-queue --queue-name my-queue
```

## To create a FIFO queue

The following example creates a FIFO queue.

```bash
aws sqs create-queue
    --queue-name my-queue
    --attributes '{"FifoQueue":"true","ContentBasedDeduplication":"true"}'
```

## To delete a queue

The following example deletes a queue.

```bash
aws sqs delete-queue --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
```

## To purge all messages

The following example purges all messages.

```bash
aws sqs purge-queue --queue-url https://sqs.us-east-1.amazonaws.com/123456789012/my-queue
```

## To get queue attributes

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
