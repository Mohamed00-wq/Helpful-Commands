# Sharing / Permissions

> Sharing / Permissions. Part of the [AMI](../) cheatsheet.

## To view AMI sharing permissions

The following example views AMI sharing permissions.

```bash
aws ec2 describe-image-attribute --image-id ami-xxxxxxxx --attribute launchPermission
```

## To share an AMI with another account

The following example shares an AMI with another account.

```bash
aws ec2 modify-image-attribute
    --image-id ami-xxxxxxxx
    --launch-permissions "Add=[{UserId=123456789012}]"
```

## To stop sharing an AMI with an account

The following example stops sharing an AMI with an account.

```bash
aws ec2 modify-image-attribute
    --image-id ami-xxxxxxxx
    --launch-permissions "Remove=[{UserId=123456789012}]"
```

## To make an AMI public

The following example makes an AMI public.

```bash
aws ec2 modify-image-attribute --image-id ami-xxxxxxxx --launch-permissions "Add=[{Group=all}]"
```
