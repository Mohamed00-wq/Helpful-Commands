# Subscription Filters

> Subscription Filters. Part of the [CloudWatch](../CloudWatch.md) cheatsheet.

## To subscribe to log events

The following example subscribes to log events.

```bash
aws logs put-subscription-filter
    --log-group-name /my/app
    --filter-name my-filter
    --destination-arn arn:aws:lambda:us-east-1:123456789012:function:my-function
    --filter-pattern ""
```

## To list subscription filters

The following example lists subscription filters.

```bash
aws logs describe-subscription-filters --log-group-name /my/app
```

## To delete subscription filter

The following example deletes subscription filter.

```bash
aws logs delete-subscription-filter --log-group-name /my/app --filter-name my-filter
```
