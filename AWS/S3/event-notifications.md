# Event Notifications

> Event Notifications. Part of the [S3](../S3.md) cheatsheet.

## To notify Lambda on object creation

The following example invokes a function whenever an object is created in the
bucket. The Lambda execution role needs `s3.amazonaws.com` in its trust policy.

```bash
aws s3api put-bucket-notification-configuration \
    --bucket <bucket> \
    --notification-configuration '{
      "LambdaFunctionConfigurations": [{
        "LambdaFunctionArn": "arn:aws:lambda:<region>:<account-id>:function:<function>",
        "Events": ["s3:ObjectCreated:*"]
      }]
    }'
```

## To check the notification configuration

The following example returns the current event notification settings.

```bash
aws s3api get-bucket-notification-configuration --bucket <bucket>
```
