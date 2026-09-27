# Mount Targets

> Mount Targets. Part of the [EFS](../EFS.md) cheatsheet.

A file system is unreachable until it has at least one mount target, and the
mount target is what carries the subnet, the network interface, and the security
groups. A regional file system wants one mount target per Availability Zone for
availability, while a One Zone file system takes exactly one in its own
Availability Zone.

## To create a mount target in a subnet

The following example creates one mount target in a subnet.

```bash
aws efs create-mount-target \
    --file-system-id <file-system-id> \
    --subnet-id <subnet-id>
```

## To create a mount target with security groups

The following example creates a mount target and attaches the security groups to
its network interface.

```bash
aws efs create-mount-target \
    --file-system-id <file-system-id> \
    --subnet-id <subnet-id> \
    --security-groups <security-group-id>
```

## To list the mount targets of a file system

The following example lists the mount targets of a file system along with the
Availability Zone each one serves.

```bash
aws efs describe-mount-targets --file-system-id <file-system-id>
```

## To read the address of a mount target

The following example prints the Availability Zone and the address of every
mount target. `IpAddress` is documented as an IPv4 address, not as a name.

```bash
aws efs describe-mount-targets --file-system-id <file-system-id> \
    --query 'MountTargets[].[AvailabilityZoneName,IpAddress]' --output text
```

## To get the DNS name of a file system

The following example reads the two fields that decide the DNS name. A regional
file system reports no Availability Zone, and its name is
`fs-<file-system-id>.efs.<region>.amazonaws.com`. A One Zone file system
reports an Availability Zone, and its name puts that zone in front of `efs`, so
it reads `fs-<file-system-id>.<availability-zone>.efs.<region>.amazonaws.com`.
The mount helper derives the same name from the file system id, which is why
passing the id is usually enough on the command line.

```bash
aws efs describe-file-systems --file-system-id <file-system-id> \
    --query 'FileSystems[0].[Name,AvailabilityZoneName]' --output text
```

## To delete a mount target

The following example deletes a mount target, and the file system cannot be
deleted until the last one is gone.

```bash
aws efs delete-mount-target --mount-target-id <mount-target-id>
```
