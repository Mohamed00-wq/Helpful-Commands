# EFS (Elastic File System)

> Commands for file systems, mount targets, security groups, access points, and
> mounting a file system on an instance.

EFS is a POSIX file system that grows as you write to it, and clients reach it
over NFSv4 rather than attaching it like a block device. The service command is
`aws efs`, which is the only name the AWS CLI v2 accepts;
`aws elasticfilesystem` is not a valid service name.

EFS bills for the storage a file system holds and for any throughput you
provision, and the provisioned amount is charged whether the workload drives
that throughput or not. Throughput and performance modes have to fit the
deployment type you picked: One Zone file systems always use General Purpose
performance mode, Max I/O is not available on One Zone or with Elastic
throughput, and Provisioned mode requires
`--provisioned-throughput-in-mibps`. A mount target and the instance that mounts
it belong in the same Availability Zone, because every byte pulled across
Availability Zones is billed as EC2 regional data transfer.

## File Systems

### To create a file system

The following example creates an encrypted regional file system with Elastic
throughput, and the creation token makes the call safe to retry.

```bash
aws efs create-file-system \
    --creation-token <creation-token> \
    --performance-mode generalPurpose \
    --throughput-mode elastic \
    --encrypted \
    --tags Key=Name,Value=<name>
```

### To create an encrypted file system with your own KMS key

The following example encrypts the file system with a customer managed key
instead of the AWS managed EFS key, and only symmetric keys work.

```bash
aws efs create-file-system \
    --creation-token <creation-token> \
    --encrypted \
    --kms-key-id <kms-key-id>
```

### To create a One Zone file system

The following example creates a file system in a single Availability Zone. One
Zone supports all three throughput modes, but the file system is capped at one
mount target and at 3 GiBps of read and 1 GiBps of write throughput in total,
and it always uses General Purpose performance mode.

```bash
aws efs create-file-system \
    --creation-token <creation-token> \
    --availability-zone-name <availability-zone> \
    --performance-mode generalPurpose \
    --encrypted
```

### To create a file system with provisioned throughput

The following example reserves a fixed 1.0 MiB/s of throughput, and the
amount is charged whether the file system uses it or not.

```bash
aws efs create-file-system \
    --creation-token <creation-token> \
    --throughput-mode provisioned \
    --provisioned-throughput-in-mibps 1.0 \
    --encrypted
```

### To list file systems

### To describe one file system

The following example returns the full description of a single file system.

```bash
aws efs describe-file-systems --file-system-id <file-system-id>
```

### To check how much a file system holds

The following example returns the stored byte count, which is the number the
storage charge is calculated from.

```bash
aws efs describe-file-systems --file-system-id <file-system-id> \
    --query 'FileSystems[0].SizeInBytes.Value' --output text
```

### To update the throughput of a file system

The following example switches a file system to provisioned throughput. After
the mode switch or an increase, EFS blocks switching back to Elastic or
Bursting and blocks lowering the amount for 24 hours.

```bash
aws efs update-file-system \
    --file-system-id <file-system-id> \
    --throughput-mode provisioned \
    --provisioned-throughput-in-mibps 1.0
```

### To tag a file system

The following example adds a name tag to a file system, and tags are how you
find resources in a multi-account estate.

```bash
aws efs tag-resource --resource-id <file-system-id> --tags Key=Name,Value=<name>
```

### To page through file systems

The following example caps each page at 100 file systems, and the CLI follows
the marker on its own until every file system is returned.

```bash
aws efs describe-file-systems --max-items 100
```

### To delete a file system

The following example deletes a file system, which fails while any mount target
still exists. No EFS operation takes a dry run flag, so confirm the id with
`describe-file-systems` first.

```bash
aws efs delete-file-system --file-system-id <file-system-id>
```

## Mount Targets

A file system is unreachable until it has at least one mount target, and the
mount target is what carries the subnet, the network interface, and the security
groups. A regional file system wants one mount target per Availability Zone for
availability, while a One Zone file system takes exactly one in its own
Availability Zone.

### To create a mount target in a subnet

The following example creates one mount target in a subnet.

```bash
aws efs create-mount-target \
    --file-system-id <file-system-id> \
    --subnet-id <subnet-id>
```

### To create a mount target with security groups

The following example creates a mount target and attaches the security groups to
its network interface.

```bash
aws efs create-mount-target \
    --file-system-id <file-system-id> \
    --subnet-id <subnet-id> \
    --security-groups <security-group-id>
```

### To list the mount targets of a file system

The following example lists the mount targets of a file system along with the
Availability Zone each one serves.

```bash
aws efs describe-mount-targets --file-system-id <file-system-id>
```

### To read the address of a mount target

The following example prints the Availability Zone and the address of every
mount target. `IpAddress` is documented as an IPv4 address, not as a name.

```bash
aws efs describe-mount-targets --file-system-id <file-system-id> \
    --query 'MountTargets[].[AvailabilityZoneName,IpAddress]' --output text
```

### To get the DNS name of a file system

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

### To delete a mount target

The following example deletes a mount target, and the file system cannot be
deleted until the last one is gone.

```bash
aws efs delete-mount-target --mount-target-id <mount-target-id>
```

## Security Groups

EFS has no security groups on the file system itself: the groups attach to the
network interface of each mount target, and the file system API never accepts
them. An access point inherits the security groups of the mount target it is
reached through rather than carrying its own. The only rule EFS traffic needs is
inbound NFS on TCP port 2049.

### To allow NFS from a client security group

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

### To attach security groups to a mount target

The following example replaces the security groups on an existing mount target.

```bash
aws efs modify-mount-target-security-groups \
    --mount-target-id <mount-target-id> \
    --security-groups <security-group-id>
```

### To list the security groups on a mount target

The following example returns the security group ids on a mount target's network
interface.

```bash
aws efs describe-mount-target-security-groups \
    --mount-target-id <mount-target-id> \
    --query 'SecurityGroups[].SecurityGroupId' --output text
```

## Access Points

An access point is an entry point that enforces a POSIX user and a root
directory, so every request through it runs as that user and sees that directory
as `/`. The gotcha is the root directory: EFS creates it only when you pass
`OwnerUid`, `OwnerGid`, and `Permissions` in `CreationInfo`, and without those
three the directory is not created and every mount against it fails.

### To create an access point that enforces a user

The following example creates an access point that enforces a POSIX user without
changing the root directory.

```bash
aws efs create-access-point \
    --file-system-id <file-system-id> \
    --posix-user Uid=1000,Gid=1000 \
    --tags Key=Name,Value=<name>
```

### To create an access point that enforces a root directory

The following example creates an access point rooted at a subdirectory and
creates that directory with its ownership and mode, which is what makes the
access point mountable on a file system that does not have the path yet.

```bash
aws efs create-access-point \
    --file-system-id <file-system-id> \
    --posix-user Uid=1000,Gid=1000 \
    --root-directory 'Path=/app,CreationInfo={OwnerUid=1000,OwnerGid=1000,Permissions=0755}' \
    --tags Key=Name,Value=<name>
```

### To list the access points of a file system

The following example lists the access points on a file system.

```bash
aws efs describe-access-points --file-system-id <file-system-id>
```

### To delete an access point

The following example deletes an access point. EFS has no update operation for
one, so changing the POSIX user or the root directory means deleting it and
creating it again.

```bash
aws efs delete-access-point --access-point-id <access-point-id>
```

## Mounting from an Instance

The mount helper is the `amazon-efs-utils` package, installed with `yum` on
Amazon Linux and with `apt-get` on Ubuntu. It negotiates TLS and IAM
credentials, so the instance needs an instance profile or the mount fails
authentication.

### To install the EFS mount helper

The following example installs the mount helper on the instance.

```bash
sudo yum install -y amazon-efs-utils
```

### To mount a file system

The following example mounts a file system at a local directory.

```bash
sudo mount -t efs -o tls,iam <file-system-id>:/ <local-mount-point>
```

### To mount an access point

The following example mounts an access point, and the client sees the access
point's root directory as `/`.

```bash
sudo mount -t efs -o tls,iam,accesspoint=<access-point-id> <file-system-id>:/ <local-mount-point>
```

### To add a file system to /etc/fstab

The following example is the `/etc/fstab` entry that mounts a file system at
boot, and `_netdev` stops the boot from hanging when the network is not up yet.

```bash
<file-system-id>:/ <local-mount-point> efs _netdev,tls 0 0
```

### To mount everything in /etc/fstab

The following example mounts every file system entry, which is how you apply a
new `/etc/fstab` line without rebooting.

```bash
sudo mount -a -t efs
```

### To unmount a file system

The following example unmounts a file system, which you do before deleting it or
before resizing the volume underneath it.

```bash
sudo umount <local-mount-point>
```

## Workflows

### To create a file system with a mount target in every Availability Zone

1. Create the file system and capture its id. Everything below needs the value
   that this call returns.

   ```bash
   FILE_SYSTEM_ID=$(aws efs create-file-system \
       --creation-token <creation-token> \
       --performance-mode generalPurpose \
       --throughput-mode elastic \
       --encrypted \
       --query 'FileSystemId' --output text)
   echo "$FILE_SYSTEM_ID"
   ```

2. Create one mount target per subnet. A regional file system stays reachable
   through whichever Availability Zone the client is in.

   ```bash
   for subnet in <subnet-id> <subnet-id> <subnet-id>; do
       aws efs create-mount-target \
           --file-system-id "$FILE_SYSTEM_ID" \
           --subnet-id "$subnet" \
           --security-groups <security-group-id>
   done
   ```

3. Confirm every mount target has left the `creating` state. A mount against a
   mount target that is still creating fails the NFS handshake.

   ```bash
   aws efs describe-mount-targets --file-system-id "$FILE_SYSTEM_ID" \
       --query 'MountTargets[].[AvailabilityZoneName,LifeCycleState]' \
       --output table
   ```

### To mount a file system on an instance over SSH

1. Capture the file system id and confirm the mount target in the instance's own
   Availability Zone is out of the `creating` state, so the mount does not fail
   the NFS handshake.

   ```bash
   FILE_SYSTEM_ID=$(aws efs describe-file-systems \
       --file-system-id <file-system-id> \
       --query 'FileSystems[0].FileSystemId' --output text)
   aws efs describe-mount-targets --file-system-id "$FILE_SYSTEM_ID" \
       --query 'MountTargets[].[AvailabilityZoneName,LifeCycleState]' \
       --output table
   ```

2. Install the mount helper on the instance and mount through the captured id,
   which the helper expands to the file system's DNS name.

   ```bash
   ssh <instance-id> 'sudo yum install -y amazon-efs-utils'
   ssh <instance-id> "sudo mkdir -p <local-mount-point> && sudo mount -t efs -o tls,iam $FILE_SYSTEM_ID:/ <local-mount-point>"
   ```

3. Record the mount in `/etc/fstab` so the file system comes back after a
   reboot.

   ```bash
   ssh <instance-id> "echo '$FILE_SYSTEM_ID:/ <local-mount-point> efs _netdev,tls 0 0' | sudo tee -a /etc/fstab"
   ```

4. Check what is actually mounted on the instance.

   ```bash
   ssh <instance-id> 'df -hT <local-mount-point>'
   ```
