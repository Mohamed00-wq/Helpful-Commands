# Access Points

> Access Points. Part of the [S3](../) cheatsheet.

## To create an access point

The following example creates an access point for the account, which gives
your applications a stable endpoint that survives bucket renames.

```bash
aws s3control create-access-point \
    --account-id <account-id> \
    --name my-access-point \
    --bucket <bucket>
```

## To list access points

The following example lists the access points in the account.

```bash
aws s3control list-access-points --account-id <account-id>
```

## To delete an access point

The following example deletes an access point.

```bash
aws s3control delete-access-point \
    --account-id <account-id> \
    --name my-access-point
```
