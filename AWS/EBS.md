# EBS (Elastic Block Store)

> Commands for volumes, snapshots, encryption, resizing, and volume types.

## Volumes

### To list all EBS volumes

### To list unattached (available) volumes

The following example lists unattached (available) volumes.

```bash
aws ec2 describe-volumes --filters "Name=status,Values=available"
```

### To list attached volumes

The following example lists attached volumes.

```bash
aws ec2 describe-volumes --filters "Name=status,Values=in-use"
```

### To get details for a specific volume

The following example gets details for a specific volume.

```bash
aws ec2 describe-volumes --volume-ids vol-xxxxxxxx
```

### To list volumes attached to a specific instance

The following example lists volumes attached to a specific instance.

```bash
aws ec2 describe-volumes --filters "Name=attachment.instance-id,Values=i-xxxxxxxx"
```

### To create a 100 GB gp3 volume

The following example creates a 100 GB gp3 volume.

```bash
aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a
```

### To create a volume from a snapshot

The following example creates a volume from a snapshot.

```bash
aws ec2 create-volume
    --snapshot-id snap-xxxxxxxx
    --volume-type gp3
    --size 100
    --availability-zone us-east-1a
```

### To create an encrypted volume

The following example creates an encrypted volume.

```bash
aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a --encrypted
```

### To delete a volume

The following example deletes a volume.

```bash
aws ec2 delete-volume --volume-id vol-xxxxxxxx
```

### To attach a volume to an instance

The following example attaches a volume to an instance.

```bash
aws ec2 attach-volume --volume-id vol-xxxxxxxx --instance-id i-xxxxxxxx --device /dev/xvdf
```

### To detach a volume from an instance

The following example detaches a volume from an instance.

```bash
aws ec2 detach-volume --volume-id vol-xxxxxxxx
```

### To resize or change volume type

The following example resizes or change volume type.

```bash
aws ec2 modify-volume --volume-id vol-xxxxxxxx --volume-type gp3 --size 200
```

### To check volume modification progress

The following example checks volume modification progress.

```bash
aws ec2 describe-volume-modifications --volume-id vol-xxxxxxxx
```

## Snapshots

### To create a snapshot from a volume

The following example creates a snapshot from a volume.

```bash
aws ec2 create-snapshot --volume-id vol-xxxxxxxx --description "backup"
```

### To list your snapshots

The following example lists your snapshots.

```bash
aws ec2 describe-snapshots --owner-ids self
```

### To get details for a specific snapshot

The following example gets details for a specific snapshot.

```bash
aws ec2 describe-snapshots --snapshot-ids snap-xxxxxxxx
```

### To delete a snapshot

The following example deletes a snapshot.

```bash
aws ec2 delete-snapshot --snapshot-id snap-xxxxxxxx
```

### To copy a snapshot to another region

The following example copies a snapshot to another region.

```bash
aws ec2 copy-snapshot
    --source-region us-east-1
    --source-snapshot-id snap-xxxxxxxx
    --destination-region us-west-2
```

### To view snapshot sharing permissions

The following example views snapshot sharing permissions.

```bash
aws ec2 describe-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute createVolumePermission
```

### To share a snapshot with an account

The following example shares a snapshot with an account.

```bash
aws ec2 modify-snapshot-attribute
    --snapshot-id snap-xxxxxxxx
    --attribute createVolumePermission
    --operation-type add
    --user-ids 123456789012
```

### To make a snapshot public

The following example makes a snapshot public.

```bash
aws ec2 modify-snapshot-attribute
    --snapshot-id snap-xxxxxxxx
    --attribute createVolumePermission
    --operation-type add
    --group-names all
```

### To stop sharing a snapshot

The following example stops sharing a snapshot.

```bash
aws ec2 modify-snapshot-attribute
    --snapshot-id snap-xxxxxxxx
    --attribute createVolumePermission
    --operation-type remove
    --user-ids 123456789012
```

### To view product codes on a snapshot

The following example views product codes on a snapshot.

```bash
aws ec2 describe-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute productCodes
```

## Encryption

### To enable default EBS encryption for the region

The following example enables default EBS encryption for the region.

```bash
aws ec2 enable-ebs-encryption-by-default
```

### To disable default EBS encryption

The following example disables default EBS encryption.

```bash
aws ec2 disable-ebs-encryption-by-default
```

### To check if default EBS encryption is enabled

The following example checks if default EBS encryption is enabled.

```bash
aws ec2 describe-ebs-encryption-by-default
```

### To list KMS keys (for custom encryption)

The following example lists KMS keys (for custom encryption).

```bash
aws ec2 describe-kms-aliases
```

## Tags

### To list all tags

The following example lists all tags.

```bash
aws ec2 describe-tags
```

### To tag an EBS volume

The following example tags an EBS volume.

```bash
aws ec2 create-tags --resources vol-xxxxxxxx --tags Key=Name,Value=my-volume
```

### To remove a tag from a volume

The following example removes a tag from a volume.

```bash
aws ec2 delete-tags --resources vol-xxxxxxxx --tags Key=Name
```

## Volume Types Reference

| Type | Use Case | Max IOPS | Max Throughput |
| `gp3` | General purpose (latest) | 16,000 | 1,000 MB/s |
| `gp2` | General purpose (legacy) | 16,000 | 250 MB/s |
| `io2` | Mission-critical, high IOPS | 64,000 | 1,000 MB/s |
| `io1` | High IOPS (legacy) | 64,000 | 1,000 MB/s |
| `st1` | Throughput-heavy (big data) | 500 | 500 MB/s |
| `sc1` | Cold storage, infrequent access | 250 | 250 MB/s |
| `standard` | Previous gen | 40 | 40 MB/s |
