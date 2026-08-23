# ☁️ AWS CLI Cheatsheet

> Essential AWS CLI commands for EC2, S3, IAM, VPC, and general configuration — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## 🖥️ EC2

| Command | Description |
|---|---|
| `aws ec2 describe-instances` | List EC2 instances and their details |
| `aws ec2 describe-vpcs` | List VPCs in the account |
| `aws ec2 describe-subnets` | List subnets |
| `aws ec2 describe-security-groups` | List security groups and their rules |
| `aws ec2 describe-route-tables` | List route tables |

```bash
aws ec2 describe-instances
aws ec2 describe-vpcs
aws ec2 describe-subnets
aws ec2 describe-security-groups
aws ec2 describe-route-tables
```

---

## 🪣 S3

| Command | Description |
|---|---|
| `aws s3 ls` | List S3 buckets |
| `aws s3 cp file.txt s3://bucket-name/` | Upload a file to a bucket |
| `aws s3 sync ./local-dir s3://bucket-name/` | Sync a local directory to a bucket |
| `aws s3api get-bucket-policy --bucket bucket-name` | Retrieve a bucket's policy |

```bash
aws s3 ls
aws s3 cp file.txt s3://bucket-name/
aws s3 sync ./local-dir s3://bucket-name/
aws s3api get-bucket-policy --bucket bucket-name
```

---

## 🔐 IAM

| Command | Description |
|---|---|
| `aws iam list-users` | List IAM users |
| `aws iam list-roles` | List IAM roles |
| `aws sts get-caller-identity` | Show the currently authenticated identity (great for debugging) |

```bash
aws iam list-users
aws iam list-roles
aws sts get-caller-identity
```

---

## 🌐 VPC

| Command | Description |
|---|---|
| `aws ec2 describe-nat-gateways` | List NAT gateways |
| `aws ec2 describe-internet-gateways` | List internet gateways |
| `aws ec2 describe-network-acls` | List network ACLs |

```bash
aws ec2 describe-nat-gateways
aws ec2 describe-internet-gateways
aws ec2 describe-network-acls
```

---

## ⚙️ General

| Command | Description |
|---|---|
| `aws configure` | Set up credentials and default region |
| `aws configure list` | Show current configuration |
| `aws <service> help` | Get help for any service |

```bash
aws configure
aws configure list
aws <service> help
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.