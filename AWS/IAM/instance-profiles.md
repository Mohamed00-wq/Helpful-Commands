# Instance Profiles

> Instance Profiles. Part of the [IAM](../IAM.md) cheatsheet.

## To list instance profiles

The following example lists instance profiles.

```bash
aws iam list-instance-profiles
```

## To get details for a specific profile

The following example gets details for a specific profile.

```bash
aws iam get-instance-profile --instance-profile-name my-profile
```

## To create an instance profile

The following example creates an instance profile.

```bash
aws iam create-instance-profile --instance-profile-name my-profile
```

## To delete an instance profile

The following example deletes an instance profile.

```bash
aws iam delete-instance-profile --instance-profile-name my-profile
```

## To attach a role to an instance profile

The following example attaches a role to an instance profile.

```bash
aws iam add-role-to-instance-profile --instance-profile-name my-profile --role-name my-role
```

## To remove a role from an instance profile

The following example removes a role from an instance profile.

```bash
aws iam remove-role-from-instance-profile --instance-profile-name my-profile --role-name my-role
```
