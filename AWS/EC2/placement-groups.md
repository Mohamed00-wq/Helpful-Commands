# Placement Groups

> Placement Groups. Part of the [EC2](../) cheatsheet.

## To list placement groups

The following example lists placement groups.

```bash
aws ec2 describe-placement-groups
```

## To create a cluster placement group

The following example creates a cluster placement group.

```bash
aws ec2 create-placement-group --group-name my-pg --strategy cluster
```

## To delete a placement group

The following example deletes a placement group.

```bash
aws ec2 delete-placement-group --group-name my-pg
```
