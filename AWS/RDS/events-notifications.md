# Events & Notifications

> Events & Notifications. Part of the [RDS](../RDS.md) cheatsheet.

## To list RDS events

The following example lists RDS events.

```bash
aws rds describe-events --source-type db-instance --start-time 2026-01-15T00:00:00Z
```

## To list event subscriptions

The following example lists event subscriptions.

```bash
aws rds describe-event-subscriptions
```

## To create event subscription

The following example creates event subscription.

```bash
aws rds create-event-subscription
    --subscription-name my-sub
    --sns-topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --source-type db-instance
```

## To delete event subscription

The following example deletes event subscription.

```bash
aws rds delete-event-subscription --subscription-name my-sub
```
