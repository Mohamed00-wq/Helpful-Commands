# Users

> Users. Part of the [IAM](../) cheatsheet.

## To list all IAM users

The following example lists all IAM users.

```bash
aws iam list-users
```

## To get details for a specific user

The following example gets details for a specific user.

```bash
aws iam get-user --user-name alice
```

## To create a new IAM user

The following example creates a new IAM user.

```bash
aws iam create-user --user-name bob
```

## To delete an IAM user

The following example deletes an IAM user.

```bash
aws iam delete-user --user-name bob
```

## To generate access keys for a user

The following example generates access keys for a user.

```bash
aws iam create-access-key --user-name bob
```

## To list access keys for a user

The following example lists access keys for a user.

```bash
aws iam list-access-keys --user-name bob
```

## To delete an access key

The following example deletes an access key.

```bash
aws iam delete-access-key --user-name bob --access-key-id AKIA...
```

## To deactivate an access key

The following example deactivates an access key.

```bash
aws iam update-access-key --user-name bob --access-key-id AKIA... --status Inactive
```

## To create a console login profile

The following example creates a console login profile.

```bash
aws iam create-login-profile --user-name bob --password P@ssw0rd!
```

## To update console password

The following example updates console password.

```bash
aws iam update-login-profile --user-name bob --password P@ssw0rd!
```

## To remove console access

The following example removes console access.

```bash
aws iam delete-login-profile --user-name bob
```
