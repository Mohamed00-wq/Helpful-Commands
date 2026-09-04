# 🖥️ EC2 (Elastic Compute Cloud)

> Essential EC2 CLI commands for managing instances, networking, and security — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Instances

| Command | Description |
|---|---|
| `aws ec2 describe-instances` | List all EC2 instances and their details |
| `aws ec2 describe-instances --instance-ids i-xxxxxxxx` | Get details for a specific instance |
| `aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"` | List only running instances |
| `aws ec2 describe-instances --filters "Name=tag:Name,Values=web-server"` | List instances by tag |
| `aws ec2 run-instances --image-id ami-xxxxxxxx --instance-type t2.micro --key-name my-key --security-group-ids sg-xxxxxxxx --subnet-id subnet-xxxxxxxx` | Launch a new instance |
| `aws ec2 start-instances --instance-ids i-xxxxxxxx` | Start a stopped instance |
| `aws ec2 stop-instances --instance-ids i-xxxxxxxx` | Stop a running instance |
| `aws ec2 terminate-instances --instance-ids i-xxxxxxxx` | Terminate (delete) an instance |
| `aws ec2 reboot-instances --instance-ids i-xxxxxxxx` | Reboot an instance |
| `aws ec2 describe-instance-status --instance-ids i-xxxxxxxx` | Check instance status |
| `aws ec2 wait instance-running --instance-ids i-xxxxxxxx` | Wait until instance is running |

```bash
aws ec2 describe-instances
aws ec2 describe-instances --instance-ids i-xxxxxxxx
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
aws ec2 describe-instances --filters "Name=tag:Name,Values=web-server"
aws ec2 run-instances \
  --image-id ami-xxxxxxxx \
  --instance-type t2.micro \
  --key-name my-key \
  --security-group-ids sg-xxxxxxxx \
  --subnet-id subnet-xxxxxxxx
aws ec2 start-instances --instance-ids i-xxxxxxxx
aws ec2 stop-instances --instance-ids i-xxxxxxxx
aws ec2 terminate-instances --instance-ids i-xxxxxxxx
aws ec2 reboot-instances --instance-ids i-xxxxxxxx
aws ec2 describe-instance-status --instance-ids i-xxxxxxxx
aws ec2 wait instance-running --instance-ids i-xxxxxxxx
```

---

## Instance Types

| Command | Description |
|---|---|
| `aws ec2 describe-instance-types` | List all available instance types |
| `aws ec2 describe-instance-types --filters "Name=instance-type,Values=t2.*"` | Filter instance types by name |
| `aws ec2 describe-instance-type-offerings --location-type availability-zone --region us-east-1` | List instance type offerings per AZ |

```bash
aws ec2 describe-instance-types
aws ec2 describe-instance-types --filters "Name=instance-type,Values=t2.*"
aws ec2 describe-instance-type-offerings \
  --location-type availability-zone \
  --region us-east-1
```

---

## Key Pairs

| Command | Description |
|---|---|
| `aws ec2 describe-key-pairs` | List all key pairs |
| `aws ec2 create-key-pair --key-name my-key` | Create a new key pair |
| `aws ec2 delete-key-pair --key-name my-key` | Delete a key pair |
| `aws ec2 import-key-pair --key-name my-key --public-key-material fileb://~/.ssh/id_rsa.pub` | Import a public key |

```bash
aws ec2 describe-key-pairs
aws ec2 create-key-pair --key-name my-key
aws ec2 delete-key-pair --key-name my-key
aws ec2 import-key-pair --key-name my-key --public-key-material fileb://~/.ssh/id_rsa.pub
```

---

## Security Groups

| Command | Description |
|---|---|
| `aws ec2 describe-security-groups` | List all security groups |
| `aws ec2 describe-security-groups --group-ids sg-xxxxxxxx` | Get details for a specific SG |
| `aws ec2 create-security-group --group-name my-sg --description "My SG" --vpc-id vpc-xxxxxxxx` | Create a new security group |
| `aws ec2 delete-security-group --group-id sg-xxxxxxxx` | Delete a security group |
| `aws ec2 authorize-security-group-ingress --group-id sg-xxxxxxxx --protocol tcp --port 80 --cidr 0.0.0.0/0` | Add an inbound rule |
| `aws ec2 authorize-security-group-egress --group-id sg-xxxxxxxx --protocol tcp --port 443 --cidr 0.0.0.0/0` | Add an outbound rule |
| `aws ec2 revoke-security-group-ingress --group-id sg-xxxxxxxx --protocol tcp --port 80 --cidr 0.0.0.0/0` | Remove an inbound rule |
| `aws ec2 revoke-security-group-egress --group-id sg-xxxxxxxx --protocol tcp --port 443 --cidr 0.0.0.0/0` | Remove an outbound rule |

```bash
aws ec2 describe-security-groups
aws ec2 create-security-group \
  --group-name my-sg \
  --description "My SG" \
  --vpc-id vpc-xxxxxxxx
aws ec2 authorize-security-group-ingress \
  --group-id sg-xxxxxxxx \
  --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-egress \
  --group-id sg-xxxxxxxx \
  --protocol tcp --port 443 --cidr 0.0.0.0/0
aws ec2 revoke-security-group-ingress \
  --group-id sg-xxxxxxxx \
  --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 revoke-security-group-egress \
  --group-id sg-xxxxxxxx \
  --protocol tcp --port 443 --cidr 0.0.0.0/0
```

---

## Elastic IPs

| Command | Description |
|---|---|
| `aws ec2 describe-addresses` | List all Elastic IPs |
| `aws ec2 allocate-address --domain vpc` | Allocate a new Elastic IP |
| `aws ec2 associate-address --instance-id i-xxxxxxxx --allocation-id eipalloc-xxxxxxxx` | Associate an EIP with an instance |
| `aws ec2 disassociate-address --association-id eipassoc-xxxxxxxx` | Disassociate an EIP |
| `aws ec2 release-address --allocation-id eipalloc-xxxxxxxx` | Release an Elastic IP |

```bash
aws ec2 describe-addresses
aws ec2 allocate-address --domain vpc
aws ec2 associate-address --instance-id i-xxxxxxxx --allocation-id eipalloc-xxxxxxxx
aws ec2 disassociate-address --association-id eipassoc-xxxxxxxx
aws ec2 release-address --allocation-id eipalloc-xxxxxxxx
```

---

## VPC, Subnets & Route Tables

| Command | Description |
|---|---|
| `aws ec2 describe-vpcs` | List VPCs in the account |
| `aws ec2 describe-subnets` | List subnets |
| `aws ec2 describe-route-tables` | List route tables |
| `aws ec2 describe-nat-gateways` | List NAT gateways |
| `aws ec2 describe-internet-gateways` | List internet gateways |
| `aws ec2 describe-network-acls` | List network ACLs |

```bash
aws ec2 describe-vpcs
aws ec2 describe-subnets
aws ec2 describe-route-tables
aws ec2 describe-nat-gateways
aws ec2 describe-internet-gateways
aws ec2 describe-network-acls
```

---

## Tags

| Command | Description |
|---|---|
| `aws ec2 describe-tags` | List all tags |
| `aws ec2 create-tags --resources i-xxxxxxxx --tags Key=Name,Value=web-server` | Add a tag to a resource |
| `aws ec2 delete-tags --resources i-xxxxxxxx --tags Key=Name` | Remove a tag from a resource |

```bash
aws ec2 describe-tags
aws ec2 create-tags --resources i-xxxxxxxx --tags Key=Name,Value=web-server
aws ec2 delete-tags --resources i-xxxxxxxx --tags Key=Name
```

---

## Placement Groups

| Command | Description |
|---|---|
| `aws ec2 describe-placement-groups` | List placement groups |
| `aws ec2 create-placement-group --group-name my-pg --strategy cluster` | Create a cluster placement group |
| `aws ec2 delete-placement-group --group-name my-pg` | Delete a placement group |

```bash
aws ec2 describe-placement-groups
aws ec2 create-placement-group --group-name my-pg --strategy cluster
aws ec2 delete-placement-group --group-name my-pg
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
