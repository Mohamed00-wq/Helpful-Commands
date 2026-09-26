# STS (Security Token Service)

> Commands for checking who you are, assuming roles, and managing temporary
> credentials.

STS issues the short lived credentials that every other AWS call runs on. Run
`get-caller-identity` first whenever a call fails with `AccessDenied`, because
it tells you which role or user you actually ended up as.

## Identity

### To check who you are

### To check who you are in a named profile

The following example resolves the identity behind a specific profile without
switching to it.

```bash
aws sts get-caller-identity --profile <profile>
```

### To check whether a session is federated or assumed

The following example shows the session issuer, which tells you whether you are
running as a federated user, an assumed role, or a native IAM user.

```bash
aws sts get-caller-identity --query 'Arn' --output text
```

## Assuming Roles

### To assume a role

The following example assumes a role and returns temporary credentials that
last one hour.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session
```

### To use assumed credentials immediately

The following example assumes a role and exports the credentials into the
current shell, so every later `aws` call uses them.

```bash
eval "$(aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --query 'Credentials.[AccessKeyId,SecretAccessKey,SessionToken]' \
    --output text)"
```

### To assume a role for a fixed duration

The following example sets a one hour session. The maximum is 12 hours for a
role, and the role's own `MaxSessionDuration` can lower that.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --duration-seconds 3600
```

### To assume a role using an external ID

The following example supplies an external ID, which is how a third party is
given access to your account without you trusting their principal directly.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --external-id <external-id>
```

### To assume a role with a tag session

The following example attaches a session tag, which IAM policies and SCPs can
then match on.

```bash
aws sts assume-role \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --role-session-name my-session \
    --tags Key=Environment,Value=prod
```

## Web Identity Federation

### To get credentials from an OIDC token

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

## Federation

### To get a federated token

The following example returns temporary credentials for a federated user,
typically used by the AWS CLI federation endpoint.

```bash
aws sts get-federation-token --name my-federated-user
```

## Decoding Errors

### To decode an access denied message

The following example decodes the opaque string that comes back with an
`AccessDenied` error, which names the missing permission and the identity that
was denied.

```bash
aws sts decode-authorization-message --encoded-message <encoded-message>
```

### To list expired presigned request tokens

The following example lists presigned requests that have already expired but
are still being accepted because the originating credentials are still valid.

```bash
aws sts list-expired-grants
```

## Workflows

### To switch profiles for one command

The following examples run a single call against a different profile without
changing the shell's environment.

```bash
aws s3 ls --profile <profile>
aws ec2 describe-instances --profile prod --region eu-west-1
```

### To export credentials for a profile into the shell

The following example exports a profile's static credentials, which is useful
for a legacy script that cannot read profiles itself. Session tokens are not
handled here, so do not use it with temporary credentials.

```bash
source <(aws configure export-credentials --profile <profile> --format env)
```
