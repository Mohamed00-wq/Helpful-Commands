# Secrets Manager Resource Policy

> Secrets Manager Resource Policy. Part of the [KMS-Secrets](../) cheatsheet.

## To get resource policy

The following example gets resource policy.

```bash
aws secretsmanager get-resource-policy --secret-id my-secret
```

## To set resource policy

The following example sets resource policy.

```bash
aws secretsmanager put-resource-policy --secret-id my-secret --resource-policy file://policy.json
```

## To remove resource policy

The following example removes resource policy.

```bash
aws secretsmanager delete-resource-policy --secret-id my-secret
```
