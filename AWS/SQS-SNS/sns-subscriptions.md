# SNS Subscriptions

> SNS Subscriptions. Part of the [SQS-SNS](../) cheatsheet.

## To subscribe by email

The following example subscribes by email.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol email
    --notification-endpoint user@example.com
```

## To subscribe by SMS

The following example subscribes by SMS.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol sms
    --notification-endpoint +1234567890
```

## To subscribe Lambda

The following example subscribes Lambda.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol lambda
    --notification-endpoint arn:aws:lambda:us-east-1:123456789012:function:my-function
```

## To subscribe SQS

The following example subscribes SQS.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol sqs
    --notification-endpoint arn:aws:sqs:us-east-1:123456789012:my-queue
```

## To subscribe HTTPS

The following example subscribes HTTPS.

```bash
aws sns subscribe
    --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --protocol https
    --notification-endpoint https://example.com/webhook
```

## To list subscriptions

The following example lists subscriptions.

```bash
aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
```

## To unsubscribe

The following example unsubscribes .

```bash
aws sns unsubscribe --subscription-arn arn:aws:sns:us-east-1:123456789012:my-topic:xxx
```

## To confirm a subscription

The following example confirms a subscription.

```bash
aws sns confirm-subscription --topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --token xxx
```
