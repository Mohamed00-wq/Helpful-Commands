# KMS Key Policy

> KMS Key Policy. Part of the [KMS-Secrets](../KMS-Secrets.md) cheatsheet.

## To get key policy

The following example gets key policy.

```bash
aws kms get-key-policy --key-id xxx --policy-name default
```

## To set key policy

The following example sets key policy.

```bash
aws kms put-key-policy --key-id xxx --policy-name default --policy file://policy.json
```
