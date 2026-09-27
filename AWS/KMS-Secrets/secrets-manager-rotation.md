# Secrets Manager Rotation

> Secrets Manager Rotation. Part of the [KMS-Secrets](../KMS-Secrets.md) cheatsheet.

## To get rotation config

The following example gets rotation config.

```bash
aws secretsmanager describe-secret --secret-id my-secret
```

## To rotate a secret manually

The following example immediately rotates a secret instead of waiting for its schedule.

```bash
aws secretsmanager rotate-secret --secret-id my-secret
```

## To update version stage

The following example updates version stage.

```bash
aws secretsmanager update-secret-version-stage
    --secret-id my-secret
    --version-stage AWSPENDING
    --version-id xxx
```
