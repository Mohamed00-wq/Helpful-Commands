# Groups

> Groups. Part of the [IAM](../) cheatsheet.

## To list all IAM groups

The following example lists all IAM groups.

```bash
aws iam list-groups
```

## To get details for a specific group

The following example gets details for a specific group.

```bash
aws iam get-group --group-name my-group
```

## To create a new group

The following example creates a new group.

```bash
aws iam create-group --group-name my-group
```

## To delete a group

The following example deletes a group.

```bash
aws iam delete-group --group-name my-group
```

## To add a user to a group

The following example adds a user to a group.

```bash
aws iam add-user-to-group --user-name alice --group-name my-group
```

## To remove a user from a group

The following example removes a user from a group.

```bash
aws iam remove-user-from-group --user-name alice --group-name my-group
```

## To list groups a user belongs to

The following example lists groups a user belongs to.

```bash
aws iam list-groups-for-user --user-name alice
```

## To list managed policies attached to a group

The following example lists managed policies attached to a group.

```bash
aws iam list-attached-group-policies --group-name my-group
```

## To attach a managed policy to a group

The following example attaches a managed policy to a group.

```bash
aws iam attach-group-policy
    --group-name my-group
    --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

## To detach a managed policy from a group

The following example detaches a managed policy from a group.

```bash
aws iam detach-group-policy
    --group-name my-group
    --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

## To list inline policies on a group

The following example lists inline policies on a group.

```bash
aws iam list-group-policies --group-name my-group
```

## To attach an inline policy to a group

The following example attaches an inline policy to a group.

```bash
aws iam put-group-policy
    --group-name my-group
    --policy-name inline-policy
    --policy-document file://policy.json
```
