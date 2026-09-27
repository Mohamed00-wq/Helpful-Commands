# Tags

> Tags. Part of the [EC2](../EC2.md) cheatsheet.

## To list all tags

The following example lists all tags.

```bash
aws ec2 describe-tags
```

## To add a tag to a resource

The following example adds a tag to a resource.

```bash
aws ec2 create-tags --resources i-xxxxxxxx --tags Key=Name,Value=web-server
```

## To remove a tag from a resource

The following example removes a tag from a resource.

```bash
aws ec2 delete-tags --resources i-xxxxxxxx --tags Key=Name
```
