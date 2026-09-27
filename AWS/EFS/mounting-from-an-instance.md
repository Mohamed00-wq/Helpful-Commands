# Mounting from an Instance

> Mounting from an Instance. Part of the [EFS](../EFS.md) cheatsheet.

The mount helper is the `amazon-efs-utils` package, installed with `yum` on
Amazon Linux and with `apt-get` on Ubuntu. It negotiates TLS and IAM
credentials, so the instance needs an instance profile or the mount fails
authentication.

## To install the EFS mount helper

The following example installs the mount helper on the instance.

```bash
sudo yum install -y amazon-efs-utils
```

## To mount a file system

The following example mounts a file system at a local directory.

```bash
sudo mount -t efs -o tls,iam <file-system-id>:/ <local-mount-point>
```

## To mount an access point

The following example mounts an access point, and the client sees the access
point's root directory as `/`.

```bash
sudo mount -t efs -o tls,iam,accesspoint=<access-point-id> <file-system-id>:/ <local-mount-point>
```

## To add a file system to /etc/fstab

The following example is the `/etc/fstab` entry that mounts a file system at
boot, and `_netdev` stops the boot from hanging when the network is not up yet.

```bash
<file-system-id>:/ <local-mount-point> efs _netdev,tls 0 0
```

## To mount everything in /etc/fstab

The following example mounts every file system entry, which is how you apply a
new `/etc/fstab` line without rebooting.

```bash
sudo mount -a -t efs
```

## To unmount a file system

The following example unmounts a file system, which you do before deleting it or
before resizing the volume underneath it.

```bash
sudo umount <local-mount-point>
```
