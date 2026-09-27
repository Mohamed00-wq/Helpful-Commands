# KMS Grants

> KMS Grants. Part of the [KMS-Secrets](../KMS-Secrets.md) cheatsheet.

## To list grants on a key

The following example lists grants on a key.

```bash
aws kms list-grants --key-id xxx
```

## To create a grant

The following example creates a grant.

```bash
aws kms create-grant
    --key-id xxx
    --grantee-principal arn:aws:iam::ACCOUNT:role/my-role
    --operations Encrypt Decrypt
```

## To revoke a grant

The following example revokes a grant.

```bash
aws kms revoke-grant --key-id xxx --grant-id xxx
```
