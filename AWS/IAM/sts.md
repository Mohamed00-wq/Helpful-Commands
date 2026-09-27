# STS (Security Token Service)

> STS (Security Token Service). Part of the [IAM](../IAM.md) cheatsheet.

## To show currently authenticated identity

The following example shows currently authenticated identity.

```bash
aws sts get-caller-identity
```

## To get session token with MFA

The following example gets session token with MFA.

```bash
aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/alice --token-code 123456
```

## To assume an IAM role

The following example assumes an IAM role.

```bash
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name "session1"
```

## To assume a role with custom duration

The following example assumes a role with custom duration.

```bash
aws sts assume-role
    --role-arn arn:aws:iam::123456789012:role/my-role
    --role-session-name "session1"
    --duration-seconds 900
```

## To assume a role with external ID

The following example assumes a role with external ID.

```bash
aws sts assume-role
    --role-arn arn:aws:iam::123456789012:role/my-role
    --role-session-name "session1"
    --external-id ABCD1234
```

## To get federated user credentials

The following example gets federated user credentials.

```bash
aws sts get-federation-token --name alice --policy file://policy.json
```

## To decode an access denied error message

The following example decodes an access denied error message.

```bash
aws sts decode-authorization-message --encoded-message "..."
```
