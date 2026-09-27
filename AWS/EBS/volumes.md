# Volumes

> Volumes. Part of the [EBS](../EBS.md) cheatsheet.

## To list all EBS volumes

## To list unattached (available) volumes

The following example lists unattached (available) volumes.

```bash
aws ec2 describe-volumes --filters "Name=status,Values=available"
```

## To list attached volumes

The following example lists attached volumes.

```bash
aws ec2 describe-volumes --filters "Name=status,Values=in-use"
```

## To get details for a specific volume

The following example gets details for a specific volume.

```bash
aws ec2 describe-volumes --volume-ids vol-xxxxxxxx
```

## To list volumes attached to a specific instance

The following example lists volumes attached to a specific instance.

```bash
aws ec2 describe-volumes --filters "Name=attachment.instance-id,Values=i-xxxxxxxx"
```

## To create a 100 GB gp3 volume

The following example creates a 100 GB gp3 volume.

```bash
aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a
```

## To create a volume from a snapshot

The following example creates a volume from a snapshot.

```bash
aws ec2 create-volume
    --snapshot-id snap-xxxxxxxx
    --volume-type gp3
    --size 100
    --availability-zone us-east-1a
```

## To create an encrypted volume

The following example creates an encrypted volume.

```bash
aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a --encrypted
```

## To delete a volume

The following example deletes a volume.

```bash
aws ec2 delete-volume --volume-id vol-xxxxxxxx
```

## To attach a volume to an instance

The following example attaches a volume to an instance.

```bash
aws ec2 attach-volume --volume-id vol-xxxxxxxx --instance-id i-xxxxxxxx --device /dev/xvdf
```

## To detach a volume from an instance

The following example detaches a volume from an instance.

```bash
aws ec2 detach-volume --volume-id vol-xxxxxxxx
```

## To resize or change volume type

The following example resizes or change volume type.

```bash
aws ec2 modify-volume --volume-id vol-xxxxxxxx --volume-type gp3 --size 200
```

## To check volume modification progress

The following example checks volume modification progress.

```bash
aws ec2 describe-volume-modifications --volume-id vol-xxxxxxxx
```
