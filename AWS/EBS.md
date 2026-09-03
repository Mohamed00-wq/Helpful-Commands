# 💾 EBS (Elastic Block Store)

> Essential EBS CLI commands for volumes, snapshots, encryption, and resizing — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Volumes

| Command | Description |
|---|---|
| `aws ec2 describe-volumes` | List all EBS volumes |
| `aws ec2 describe-volumes --filters "Name=status,Values=available"` | List unattached (available) volumes |
| `aws ec2 describe-volumes --filters "Name=status,Values=in-use"` | List attached volumes |
| `aws ec2 describe-volumes --volume-ids vol-xxxxxxxx` | Get details for a specific volume |
| `aws ec2 describe-volumes --filters "Name=attachment.instance-id,Values=i-xxxxxxxx"` | List volumes attached to a specific instance |
| `aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a` | Create a 100 GB gp3 volume |
| `aws ec2 create-volume --snapshot-id snap-xxxxxxxx --volume-type gp3 --size 100 --availability-zone us-east-1a` | Create a volume from a snapshot |
| `aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a --encrypted` | Create an encrypted volume |
| `aws ec2 delete-volume --volume-id vol-xxxxxxxx` | Delete a volume |
| `aws ec2 attach-volume --volume-id vol-xxxxxxxx --instance-id i-xxxxxxxx --device /dev/xvdf` | Attach a volume to an instance |
| `aws ec2 detach-volume --volume-id vol-xxxxxxxx` | Detach a volume from an instance |
| `aws ec2 modify-volume --volume-id vol-xxxxxxxx --volume-type gp3 --size 200` | Resize or change volume type |
| `aws ec2 describe-volume-modifications --volume-id vol-xxxxxxxx` | Check volume modification progress |

```bash
# List volumes
aws ec2 describe-volumes
aws ec2 describe-volumes --filters "Name=status,Values=available"
aws ec2 describe-volumes --filters "Name=status,Values=in-use"
aws ec2 describe-volumes --filters "Name=attachment.instance-id,Values=i-xxxxxxxx"

# Create volumes
aws ec2 create-volume --volume-type gp3 --size 100 --availability-zone us-east-1a
aws ec2 create-volume \
  --snapshot-id snap-xxxxxxxx \
  --volume-type gp3 --size 100 --availability-zone us-east-1a
aws ec2 create-volume \
  --volume-type gp3 --size 100 \
  --availability-zone us-east-1a --encrypted

# Attach / Detach
aws ec2 attach-volume \
  --volume-id vol-xxxxxxxx \
  --instance-id i-xxxxxxxx \
  --device /dev/xvdf
aws ec2 detach-volume --volume-id vol-xxxxxxxx

# Resize / Modify
aws ec2 modify-volume --volume-id vol-xxxxxxxx --volume-type gp3 --size 200
aws ec2 describe-volume-modifications --volume-id vol-xxxxxxxx

# Delete
aws ec2 delete-volume --volume-id vol-xxxxxxxx
```

---

## Snapshots

| Command | Description |
|---|---|
| `aws ec2 create-snapshot --volume-id vol-xxxxxxxx --description "backup"` | Create a snapshot from a volume |
| `aws ec2 describe-snapshots --owner-ids self` | List your snapshots |
| `aws ec2 describe-snapshots --snapshot-ids snap-xxxxxxxx` | Get details for a specific snapshot |
| `aws ec2 delete-snapshot --snapshot-id snap-xxxxxxxx` | Delete a snapshot |
| `aws ec2 copy-snapshot --source-region us-east-1 --source-snapshot-id snap-xxxxxxxx --destination-region us-west-2` | Copy a snapshot to another region |
| `aws ec2 describe-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute createVolumePermission` | View snapshot sharing permissions |
| `aws ec2 modify-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute createVolumePermission --operation-type add --user-ids 123456789012` | Share a snapshot with an account |
| `aws ec2 modify-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute createVolumePermission --operation-type add --group-names all` | Make a snapshot public |
| `aws ec2 modify-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute createVolumePermission --operation-type remove --user-ids 123456789012` | Stop sharing a snapshot |
| `aws ec2 describe-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute productCodes` | View product codes on a snapshot |

```bash
# Snapshots
aws ec2 create-snapshot --volume-id vol-xxxxxxxx --description "backup"
aws ec2 describe-snapshots --owner-ids self
aws ec2 delete-snapshot --snapshot-id snap-xxxxxxxx
aws ec2 copy-snapshot \
  --source-region us-east-1 \
  --source-snapshot-id snap-xxxxxxxx \
  --destination-region us-west-2

# Sharing
aws ec2 describe-snapshot-attribute \
  --snapshot-id snap-xxxxxxxx \
  --attribute createVolumePermission
aws ec2 modify-snapshot-attribute \
  --snapshot-id snap-xxxxxxxx \
  --attribute createVolumePermission \
  --operation-type add --user-ids 123456789012
aws ec2 modify-snapshot-attribute \
  --snapshot-id snap-xxxxxxxx \
  --attribute createVolumePermission \
  --operation-type add --group-names all
```

---

## Encryption

| Command | Description |
|---|---|
| `aws ec2 enable-ebs-encryption-by-default` | Enable default EBS encryption for the region |
| `aws ec2 disable-ebs-encryption-by-default` | Disable default EBS encryption |
| `aws ec2 describe-ebs-encryption-by-default` | Check if default EBS encryption is enabled |
| `aws ec2 describe-kms-aliases` | List KMS keys (for custom encryption) |

```bash
aws ec2 enable-ebs-encryption-by-default
aws ec2 disable-ebs-encryption-by-default
aws ec2 describe-ebs-encryption-by-default
aws ec2 describe-kms-aliases
```

---

## Tags

| Command | Description |
|---|---|
| `aws ec2 describe-tags` | List all tags |
| `aws ec2 create-tags --resources vol-xxxxxxxx --tags Key=Name,Value=my-volume` | Tag an EBS volume |
| `aws ec2 delete-tags --resources vol-xxxxxxxx --tags Key=Name` | Remove a tag from a volume |

```bash
aws ec2 describe-tags
aws ec2 create-tags --resources vol-xxxxxxxx --tags Key=Name,Value=my-volume
aws ec2 delete-tags --resources vol-xxxxxxxx --tags Key=Name
```

---

## Volume Types Reference

| Type | Use Case | Max IOPS | Max Throughput |
|---|---|---|---|
| `gp3` | General purpose (latest) | 16,000 | 1,000 MB/s |
| `gp2` | General purpose (legacy) | 16,000 | 250 MB/s |
| `io2` | Mission-critical, high IOPS | 64,000 | 1,000 MB/s |
| `io1` | High IOPS (legacy) | 64,000 | 1,000 MB/s |
| `st1` | Throughput-heavy (big data) | 500 | 500 MB/s |
| `sc1` | Cold storage, infrequent access | 250 | 250 MB/s |
| `standard` | Previous gen | 40 | 40 MB/s |

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
