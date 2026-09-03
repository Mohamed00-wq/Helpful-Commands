# 🖼️ AMI (Amazon Machine Images)

> Essential AMI CLI commands for managing machine images, sharing, and copying — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Describe / Search

| Command | Description |
|---|---|
| `aws ec2 describe-images --owners self` | List your custom AMIs |
| `aws ec2 describe-images --owners amazon` | List AWS-owned AMIs |
| `aws ec2 describe-images --filters "Name=name,Values=amzn2-ami-hvm-*"` | Search for Amazon Linux 2 AMIs |
| `aws ec2 describe-images --filters "Name=architecture,Values=x86_64"` | Filter by architecture |
| `aws ec2 describe-images --filters "Name=virtualization-type,Values=hvm"` | Filter by virtualization type |
| `aws ec2 describe-images --image-ids ami-xxxxxxxx` | Get details for a specific AMI |

```bash
aws ec2 describe-images --owners self
aws ec2 describe-images --owners amazon
aws ec2 describe-images --filters "Name=name,Values=amzn2-ami-hvm-*"
aws ec2 describe-images --filters "Name=architecture,Values=x86_64"
aws ec2 describe-images --filters "Name=virtualization-type,Values=hvm"
aws ec2 describe-images --image-ids ami-xxxxxxxx
```

---

## Create / Register

| Command | Description |
|---|---|
| `aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami" --no-reboot` | Create an AMI from a running instance (no reboot) |
| `aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami"` | Create an AMI from an instance (reboots) |
| `aws ec2 register-image --name "custom" --architecture x86_64 --root-device-name /dev/xvda --virtualization-type hvm` | Register a new AMI |

```bash
aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami" --no-reboot
aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami"
aws ec2 register-image \
  --name "custom" \
  --architecture x86_64 \
  --root-device-name /dev/xvda \
  --virtualization-type hvm
```

---

## Copy / Delete

| Command | Description |
|---|---|
| `aws ec2 copy-image --source-image-id ami-xxxxxxxx --source-region us-east-1 --destination-region us-west-2` | Copy an AMI to another region |
| `aws ec2 copy-image --source-image-id ami-xxxxxxxx --source-region us-east-1 --encrypted` | Copy an AMI with encryption |
| `aws ec2 deregister-image --image-id ami-xxxxxxxx` | Delete (deregister) an AMI |

```bash
aws ec2 copy-image \
  --source-image-id ami-xxxxxxxx \
  --source-region us-east-1 \
  --destination-region us-west-2
aws ec2 copy-image \
  --source-image-id ami-xxxxxxxx \
  --source-region us-east-1 \
  --encrypted
aws ec2 deregister-image --image-id ami-xxxxxxxx
```

---

## Sharing / Permissions

| Command | Description |
|---|---|
| `aws ec2 describe-image-attribute --image-id ami-xxxxxxxx --attribute launchPermission` | View AMI sharing permissions |
| `aws ec2 modify-image-attribute --image-id ami-xxxxxxxx --launch-permissions "Add=[{UserId=123456789012}]"` | Share an AMI with another account |
| `aws ec2 modify-image-attribute --image-id ami-xxxxxxxx --launch-permissions "Remove=[{UserId=123456789012}]"` | Stop sharing an AMI with an account |
| `aws ec2 modify-image-attribute --image-id ami-xxxxxxxx --launch-permissions "Add=[{Group=all}]"` | Make an AMI public |

```bash
aws ec2 describe-image-attribute \
  --image-id ami-xxxxxxxx \
  --attribute launchPermission
aws ec2 modify-image-attribute \
  --image-id ami-xxxxxxxx \
  --launch-permissions "Add=[{UserId=123456789012}]"
aws ec2 modify-image-attribute \
  --image-id ami-xxxxxxxx \
  --launch-permissions "Remove=[{UserId=123456789012}]"
aws ec2 modify-image-attribute \
  --image-id ami-xxxxxxxx \
  --launch-permissions "Add=[{Group=all}]"
```

---

## Block Device Mapping

| Command | Description |
|---|---|
| `aws ec2 describe-image-attribute --image-id ami-xxxxxxxx --attribute blockDeviceMapping` | View AMI block device mappings |
| `aws ec2 modify-image-attribute --image-id ami-xxxxxxxx --block-device-mappings "[{\"DeviceName\":\"/dev/xvda\",\"Ebs\":{\"VolumeSize\":50,\"VolumeType\":\"gp3\"}}]"` | Modify AMI block device mapping |

```bash
aws ec2 describe-image-attribute \
  --image-id ami-xxxxxxxx \
  --attribute blockDeviceMapping
aws ec2 modify-image-attribute \
  --image-id ami-xxxxxxxx \
  --block-device-mappings '[{"DeviceName":"/dev/xvda","Ebs":{"VolumeSize":50,"VolumeType":"gp3"}}]'
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
