# Roles

> Roles. Part of the [IAM](../) cheatsheet.

## To list all IAM roles

The following example lists all IAM roles.

```bash
aws iam list-roles
```

## To get details for a specific role

The following example gets details for a specific role.

```bash
aws iam get-role --role-name my-role
```

## To create a role with a trust policy

The following example creates a role with a trust policy.

```bash
aws iam create-role --role-name my-role --assume-role-policy-document file://trust.json
```

## To delete a role

The following example deletes a role.

```bash
aws iam delete-role --role-name my-role
```

## To update role session duration

The following example updates role session duration.

```bash
aws iam update-role --role-name my-role --max-session-duration 3600
```

## To get an inline policy from a role

The following example gets an inline policy from a role.

```bash
aws iam get-role-policy --role-name my-role --policy-name my-policy
```

## To list inline policies on a role

The following example lists inline policies on a role.

```bash
aws iam list-role-policies --role-name my-role
```

## To list managed policies attached to a role

The following example lists managed policies attached to a role.

```bash
aws iam list-attached-role-policies --role-name my-role
```

## To attach a managed policy to a role

The following example attaches a managed policy to a role.

```bash
aws iam attach-role-policy
    --role-name my-role
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

## To detach a managed policy from a role

The following example detaches a managed policy from a role.

```bash
aws iam detach-role-policy
    --role-name my-role
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

## To attach an inline policy to a role

The following example attaches an inline policy to a role.

```bash
aws iam put-role-policy
    --role-name my-role
    --policy-name inline-policy
    --policy-document file://policy.json
```

## To remove an inline policy from a role

The following example removes an inline policy from a role.

```bash
aws iam delete-role-policy --role-name my-role --policy-name inline-policy
```
