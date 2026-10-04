# Time to Live

> Time to Live. Part of the [DynamoDB](../) cheatsheet.

## To enable time to live

The following example turns on expiry on a timestamp attribute, which holds
epoch seconds as a number, because that is what DynamoDB compares against the
current time.

```bash
aws dynamodb update-time-to-live \
    --table-name <table> \
    --time-to-live-specification \
        Enabled=true,AttributeName=expiresAt
```

## To check the time to live status

The following example returns the attribute name and the current status,
which may take up to an hour to change after you enable it.

```bash
aws dynamodb describe-time-to-live --table-name <table>
```

## To disable time to live

The following example stops expiring items, and items already marked expired
are not restored.

```bash
aws dynamodb update-time-to-live \
    --table-name <table> \
    --time-to-live-specification \
        Enabled=false,AttributeName=expiresAt
```
