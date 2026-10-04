# Identity

> Identity. Part of the [STS](../) cheatsheet.

## To check who you are

## To check who you are in a named profile

The following example resolves the identity behind a specific profile without
switching to it.

```bash
aws sts get-caller-identity --profile <profile>
```

## To check whether a session is federated or assumed

The following example shows the session issuer, which tells you whether you are
running as a federated user, an assumed role, or a native IAM user.

```bash
aws sts get-caller-identity --query 'Arn' --output text
```
