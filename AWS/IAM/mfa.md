# MFA

> MFA. Part of the [IAM](../) cheatsheet.

## To list MFA devices for a user

The following example lists MFA devices for a user.

```bash
aws iam list-mfa-devices --user-name alice
```

## To enable an MFA device

The following example enables an MFA device.

```bash
aws iam enable-mfa-device
    --user-name alice
    --serial-number arn:aws:iam::123456789012:mfa/alice
    --authentication-code1 123456
    --authentication-code2 789012
```

## To deactivate an MFA device

The following example deactivates an MFA device.

```bash
aws iam deactivate-mfa-device --user-name alice --serial-number arn:aws:iam::123456789012:mfa/alice
```

## To resync an MFA device

The following example resyncs an MFA device.

```bash
aws iam resync-mfa-device
    --user-name alice
    --serial-number arn:aws:iam::123456789012:mfa/alice
    --authentication-code1 123456
    --authentication-code2 789012
```
