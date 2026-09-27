# SNS Topics

> SNS Topics. Part of the [SQS-SNS](../SQS-SNS.md) cheatsheet.

## To list all topics

The following example lists all topics.

```bash
aws sns list-topics
```

## To create a standard topic

The following example creates a standard topic.

```bash
aws sns create-topic --name my-topic
```

## To create a FIFO topic

The following example creates a FIFO topic.

```bash
aws sns create-topic --name my-topic.fifo --attributes FifoTopic=true
```

## To delete a topic

The following example deletes a topic.

```bash
aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
```

## To get topic attributes

The following example gets topic attributes.

```bash
aws sns get-topic-attributes --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
```
