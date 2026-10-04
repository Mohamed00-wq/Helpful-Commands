# Instance Types

> Instance Types. Part of the [EC2](../) cheatsheet.

## To list all available instance types

## To filter instance types by name

The following example filters instance types by name.

```bash
aws ec2 describe-instance-types --filters "Name=instance-type,Values=t3.*"
```

## To list instance type offerings per AZ

The following example lists instance type offerings per AZ.

```bash
aws ec2 describe-instance-type-offerings --location-type availability-zone --region us-east-1
```
