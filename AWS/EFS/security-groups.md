# Security Groups

> Security Groups. Part of the [EFS](../) cheatsheet.

EFS has no security groups on the file system itself: the groups attach to the
network interface of each mount target, and the file system API never accepts
them. An access point inherits the security groups of the mount target it is
reached through rather than carrying its own. The only rule EFS traffic needs is
inbound NFS on TCP port 2049.

## To allow NFS from a client security group

The following example opens port 2049 to a client security group, and the dry
run reports what would change without changing it.

```bash
aws ec2 authorize-security-group-ingress \
    --group-id <security-group-id> \
    --protocol tcp \
    --port 2049 \
    --source-group <client-security-group-id> \
    --dry-run
aws ec2 authorize-security-group-ingress \
    --group-id <security-group-id> \
    --protocol tcp \
    --port 2049 \
    --source-group <client-security-group-id>
```

## To attach security groups to a mount target

The following example replaces the security groups on an existing mount target.

```bash
aws efs modify-mount-target-security-groups \
    --mount-target-id <mount-target-id> \
    --security-groups <security-group-id>
```

## To list the security groups on a mount target

The following example returns the security group ids on a mount target's network
interface.

```bash
aws efs describe-mount-target-security-groups \
    --mount-target-id <mount-target-id> \
    --query 'SecurityGroups[].SecurityGroupId' --output text
```
