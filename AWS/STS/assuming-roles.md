# Assuming Roles

> Assuming Roles. Part of the [STS](../STS.md) cheatsheet.

## To assume a role

The following example assumes a role and returns temporary credentials that
last one hour.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session
```

## To use assumed credentials immediately

The following example assumes a role and exports the credentials into the
current shell, so every later `aws` call uses them.

```bash
eval "$(aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --query 'Credentials.[AccessKeyId,SecretAccessKey,SessionToken]' \
    --output text)"
```

## To assume a role for a fixed duration

The following example sets a one hour session. The maximum is 12 hours for a
role, and the role's own `MaxSessionDuration` can lower that.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --duration-seconds 3600
```

## To assume a role using an external ID

The following example supplies an external ID, which is how a third party is
given access to your account without you trusting their principal directly.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --external-id <external-id>
```

## To assume a role with a tag session

The following example attaches a session tag, which IAM policies and SCPs can
then match on.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --tags Key=Environment,Value=prod
```
