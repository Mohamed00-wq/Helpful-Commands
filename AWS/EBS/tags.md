# Tags

> Tags. Part of the [EBS](../) cheatsheet.

## To list all tags

The following example lists all tags.

```bash
aws ec2 describe-tags
```

## To tag an EBS volume

The following example tags an EBS volume.

```bash
aws ec2 create-tags --resources vol-xxxxxxxx --tags Key=Name,Value=my-volume
```

## To remove a tag from a volume

The following example removes a tag from a volume.

```bash
aws ec2 delete-tags --resources vol-xxxxxxxx --tags Key=Name
```
