# Web Identity Federation

> Web Identity Federation. Part of the [STS](../STS.md) cheatsheet.

## To get credentials from an OIDC token

The following example exchanges a token from an OIDC provider, which is how a
GitHub Actions workflow or a Kubernetes service account assumes a role without
any long lived keys.

```bash
aws sts assume-role-with-web-identity \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --web-identity-token <token> \
    --web-identity-provider <provider-arn>
```
