# File Systems

> File Systems. Part of the [EFS](../) cheatsheet.

## To create a file system

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

## To create an encrypted file system with your own KMS key

The following example encrypts the file system with a customer managed key
instead of the AWS managed EFS key, and only symmetric keys work.

```bash
aws efs create-file-system \
    --creation-token <creation-token> \
    --encrypted \
    --kms-key-id <kms-key-id>
```

## To create a One Zone file system

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

## To create a file system with provisioned throughput

The following example reserves a fixed 1.0 MiB/s of throughput, and the
amount is charged whether the file system uses it or not.

```bash
aws efs create-file-system \
    --creation-token <creation-token> \
    --throughput-mode provisioned \
    --provisioned-throughput-in-mibps 1.0 \
    --encrypted
```

## To list file systems

## To describe one file system

The following example returns the full description of a single file system.

```bash
aws efs describe-file-systems --file-system-id <file-system-id>
```

## To check how much a file system holds

The following example returns the stored byte count, which is the number the
storage charge is calculated from.

```bash
aws efs describe-file-systems --file-system-id <file-system-id> \
    --query 'FileSystems[0].SizeInBytes.Value' --output text
```

## To update the throughput of a file system

The following example switches a file system to provisioned throughput. After
the mode switch or an increase, EFS blocks switching back to Elastic or
Bursting and blocks lowering the amount for 24 hours.

```bash
aws efs update-file-system \
    --file-system-id <file-system-id> \
    --throughput-mode provisioned \
    --provisioned-throughput-in-mibps 1.0
```

## To tag a file system

The following example adds a name tag to a file system, and tags are how you
find resources in a multi-account estate.

```bash
aws efs tag-resource --resource-id <file-system-id> --tags Key=Name,Value=<name>
```

## To page through file systems

The following example caps each page at 100 file systems, and the CLI follows
the marker on its own until every file system is returned.

```bash
aws efs describe-file-systems --max-items 100
```

## To delete a file system

The following example deletes a file system, which fails while any mount target
still exists. No EFS operation takes a dry run flag, so confirm the id with
`describe-file-systems` first.

```bash
aws efs delete-file-system --file-system-id <file-system-id>
```
