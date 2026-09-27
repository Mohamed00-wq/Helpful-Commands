# Access Points

> Access Points. Part of the [EFS](../EFS.md) cheatsheet.

An access point is an entry point that enforces a POSIX user and a root
directory, so every request through it runs as that user and sees that directory
as `/`. The gotcha is the root directory: EFS creates it only when you pass
`OwnerUid`, `OwnerGid`, and `Permissions` in `CreationInfo`, and without those
three the directory is not created and every mount against it fails.

## To create an access point that enforces a user

The following example creates an access point that enforces a POSIX user without
changing the root directory.

```bash
aws efs create-access-point \
    --file-system-id <file-system-id> \
    --posix-user Uid=1000,Gid=1000 \
    --tags Key=Name,Value=<name>
```

## To create an access point that enforces a root directory

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

## To list the access points of a file system

The following example lists the access points on a file system.

```bash
aws efs describe-access-points --file-system-id <file-system-id>
```

## To delete an access point

The following example deletes an access point. EFS has no update operation for
one, so changing the POSIX user or the root directory means deleting it and
creating it again.

```bash
aws efs delete-access-point --access-point-id <access-point-id>
```
