# Secrets Manager

> Secrets Manager. Part of the [KMS-Secrets](../) cheatsheet.

## To list all secrets

The following example lists all secrets.

```bash
aws secretsmanager list-secrets
```

## To get secret value

The following example gets secret value.

```bash
aws secretsmanager get-secret-value --secret-id my-secret
```

## To create a secret (JSON)

The following example creates a secret (JSON).

```bash
aws secretsmanager create-secret
    --name my-secret
    --secret-string '{"username":"admin","password":"P@ssw0rd!"}'
```

## To create a secret (text)

The following example creates a secret (text).

```bash
aws secretsmanager create-secret --name my-secret --secret-string "plain-text-value"
```

## To create a binary secret

The following example creates a binary secret.

```bash
aws secretsmanager create-secret --name my-secret --secret-binary fileb://binary.dat
```

## To update a secret

The following example updates a secret.

```bash
aws secretsmanager update-secret
    --secret-id my-secret
    --secret-string '{"username":"admin","password":"NewP@ss!"}'
```

## To delete a secret

The following example deletes a secret.

```bash
aws secretsmanager delete-secret --secret-id my-secret
```

## To force delete

The following example forces delete.

```bash
aws secretsmanager delete-secret --secret-id my-secret --force-delete-without-recovery
```

## To restore a deleted secret

The following example restores a deleted secret.

```bash
aws secretsmanager restore-secret --secret-id my-secret
```

```bash
# Create secret
aws secretsmanager create-secret \
    --name my-secret \
    --secret-string '{"username":"admin","password":"P@ssw0rd!"}'

# Get secret value
aws secretsmanager get-secret-value \
    --secret-id my-secret \
    --query SecretString \
    --output text
```
