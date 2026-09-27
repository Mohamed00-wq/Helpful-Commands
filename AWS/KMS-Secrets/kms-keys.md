# KMS Keys

> KMS Keys. Part of the [KMS-Secrets](../KMS-Secrets.md) cheatsheet.

## To list all KMS keys

The following example lists all KMS keys.

```bash
aws kms list-keys
```

## To get key details

The following example gets key details.

```bash
aws kms describe-key --key-id alias/my-key
```

## To create a KMS key

The following example creates a KMS key.

```bash
aws kms create-key --description "My encryption key" --tags TagKey=env,TagValue=prod
```

## To create a symmetric key

The following example creates a symmetric key.

```bash
aws kms create-key --key-spec SYMMETRIC_DEFAULT
```

## To create an asymmetric key

The following example creates an asymmetric key.

```bash
aws kms create-key --key-spec RSA_2048 --key-usage ENCRYPT_DECRYPT
```

## To enable a key

The following example enables a key.

```bash
aws kms enable-key --key-id xxx
```

## To disable a key

The following example disables a key.

```bash
aws kms disable-key --key-id xxx
```

## To schedule key deletion

The following example schedules key deletion.

```bash
aws kms schedule-key-deletion --key-id xxx --pending-window-in-days 7
```

## To cancel scheduled deletion

The following example cancels scheduled deletion.

```bash
aws kms cancel-key-deletion --key-id xxx
```

```bash
# Create KMS key
aws kms create-key --description "My encryption key"

# Create alias
aws kms create-alias --alias-name alias/my-key --target-key-id xxx

# Schedule deletion
aws kms schedule-key-deletion \
    --key-id xxx \
    --pending-window-in-days 7
```
