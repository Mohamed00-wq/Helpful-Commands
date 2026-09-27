# User-Policy Attachments

> User-Policy Attachments. Part of the [IAM](../IAM.md) cheatsheet.

## To attach a managed policy to a user

The following example attaches a managed policy to a user.

```bash
aws iam attach-user-policy --user-name alice --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

## To detach a managed policy from a user

The following example detaches a managed policy from a user.

```bash
aws iam detach-user-policy --user-name alice --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

## To list managed policies attached to a user

The following example lists managed policies attached to a user.

```bash
aws iam list-attached-user-policies --user-name alice
```

## To list inline policies attached to a user

The following example lists inline policies attached to a user.

```bash
aws iam list-user-policies --user-name alice
```

## To attach an inline policy to a user

The following example attaches an inline policy to a user.

```bash
aws iam put-user-policy
    --user-name alice
    --policy-name inline-policy
    --policy-document file://policy.json
```

## To remove an inline policy from a user

The following example removes an inline policy from a user.

```bash
aws iam delete-user-policy --user-name alice --policy-name inline-policy
```

## To get an inline policy from a user

The following example gets an inline policy from a user.

```bash
aws iam get-user-policy --user-name alice --policy-name inline-policy
```
