# 🌐 VPC (Virtual Private Cloud)

> Advanced VPC CLI commands for subnets, route tables, NAT gateways, VPC peering, and endpoints — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## VPC Operations

| Command | Description |
|---|---|
| `aws ec2 describe-vpcs` | List all VPCs |
| `aws ec2 describe-vpcs --vpc-ids vpc-xxxxxxxx` | Get VPC details |
| `aws ec2 create-vpc --cidr-block 10.0.0.0/16` | Create a VPC |
| `aws ec2 delete-vpc --vpc-id vpc-xxxxxxxx` | Delete a VPC |
| `aws ec2 modify-vpc-attribute --vpc-id vpc-xxxxxxxx --enable-dns-support` | Enable DNS support |
| `aws ec2 modify-vpc-attribute --vpc-id vpc-xxxxxxxx --enable-dns-hostnames` | Enable DNS hostnames |
| `aws ec2 describe-vpc-attribute --vpc-id vpc-xxxxxxxx --attribute enableDnsSupport` | Check DNS support |

```bash
# Create VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Enable DNS (required for VPC endpoints)
aws ec2 modify-vpc-attribute --vpc-id vpc-xxx --enable-dns-support
aws ec2 modify-vpc-attribute --vpc-id vpc-xxx --enable-dns-hostnames
```

---

## Subnets

| Command | Description |
|---|---|
| `aws ec2 describe-subnets` | List all subnets |
| `aws ec2 describe-subnets --subnet-ids subnet-xxxxxxxx` | Get subnet details |
| `aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone us-east-1a` | Create a subnet |
| `aws ec2 delete-subnet --subnet-id subnet-xxxxxxxx` | Delete a subnet |
| `aws ec2 modify-subnet-attribute --subnet-id subnet-xxxxxxxx --map-public-ip-on-launch` | Make subnet public |
| `aws ec2 modify-subnet-attribute --subnet-id subnet-xxxxxxxx --no-map-public-ip-on-launch` | Make subnet private |
| `aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx` | Associate route table |

```bash
# Create public subnet
aws ec2 create-subnet \
  --vpc-id vpc-xxx \
  --cidr-block 10.0.1.0/24 \
  --availability-zone us-east-1a

# Enable auto-assign public IP
aws ec2 modify-subnet-attribute \
  --subnet-id subnet-xxx \
  --map-public-ip-on-launch
```

---

## Internet Gateway

| Command | Description |
|---|---|
| `aws ec2 describe-internet-gateways` | List IGWs |
| `aws ec2 create-internet-gateway` | Create an IGW |
| `aws ec2 attach-internet-gateway --internet-gateway-id igw-xxxxxxxx --vpc-id vpc-xxxxxxxx` | Attach IGW to VPC |
| `aws ec2 detach-internet-gateway --internet-gateway-id igw-xxxxxxxx --vpc-id vpc-xxxxxxxx` | Detach IGW |
| `aws ec2 delete-internet-gateway --internet-gateway-id igw-xxxxxxxx` | Delete IGW |

```bash
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway \
  --internet-gateway-id igw-xxx \
  --vpc-id vpc-xxx
```

---

## NAT Gateway

| Command | Description |
|---|---|
| `aws ec2 describe-nat-gateways` | List NAT gateways |
| `aws ec2 create-nat-gateway --subnet-id subnet-xxxxxxxx --allocation-id eipalloc-xxxxxxxx` | Create a NAT gateway |
| `aws ec2 delete-nat-gateway --nat-gateway-id nat-xxxxxxxx` | Delete a NAT gateway |
| `aws ec2 describe-nat-gateways --filter "Name=vpc-id,Values=vpc-xxx"` | List NAT gateways for a VPC |

```bash
# Allocate EIP first
aws ec2 allocate-address --domain vpc

# Create NAT gateway
aws ec2 create-nat-gateway \
  --subnet-id subnet-xxx \
  --allocation-id eipalloc-xxx
```

---

## Route Tables

| Command | Description |
|---|---|
| `aws ec2 describe-route-tables` | List route tables |
| `aws ec2 create-route-table --vpc-id vpc-xxxxxxxx` | Create a route table |
| `aws ec2 create-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxxxxxx` | Add route to IGW |
| `aws ec2 create-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0 --nat-gateway-id nat-xxxxxxxx` | Add route to NAT |
| `aws ec2 delete-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0` | Delete a route |
| `aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx` | Associate with subnet |
| `aws ec2 disassociate-route-table --association-id rtbassoc-xxxxxxxx` | Disassociate route table |

```bash
# Create route table with internet route
aws ec2 create-route-table --vpc-id vpc-xxx
aws ec2 create-route \
  --route-table-id rtb-xxx \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-xxx
aws ec2 associate-route-table \
  --route-table-id rtb-xxx \
  --subnet-id subnet-xxx
```

---

## VPC Peering

| Command | Description |
|---|---|
| `aws ec2 describe-vpc-peering-connections` | List peering connections |
| `aws ec2 create-vpc-peering-connection --vpc-id vpc-xxx --peer-vpc-id vpc-yyy` | Create peering request |
| `aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx` | Accept peering request |
| `aws ec2 reject-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx` | Reject peering |
| `aws ec2 delete-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx` | Delete peering |

```bash
# Create peering
aws ec2 create-vpc-peering-connection \
  --vpc-id vpc-xxx --peer-vpc-id vpc-yyy

# Accept (from peer VPC account)
aws ec2 accept-vpc-peering-connection \
  --vpc-peering-connection-id pcx-xxx

# Add routes (on both sides)
aws ec2 create-route \
  --route-table-id rtb-xxx \
  --destination-cidr-block 10.1.0.0/16 \
  --vpc-peering-connection-id pcx-xxx
```

---

## VPC Endpoints (PrivateLink)

| Command | Description |
|---|---|
| `aws ec2 describe-vpc-endpoints` | List all endpoints |
| `aws ec2 create-vpc-endpoint --vpc-id vpc-xxx --service-name com.amazonaws.us-east-1.s3 --route-table-ids rtb-xxx` | Create S3 gateway endpoint |
| `aws ec2 create-vpc-endpoint --vpc-id vpc-xxx --service-name com.amazonaws.us-east-1.dynamodb --route-table-ids rtb-xxx` | Create DynamoDB endpoint |
| `aws ec2 create-vpc-endpoint --vpc-id vpc-xxx --service-name com.amazonaws.us-east-1.sqs --vpc-endpoint-type Interface --subnet-ids subnet-xxx --security-group-ids sg-xxx` | Create interface endpoint |
| `aws ec2 delete-vpc-endpoints --vpc-endpoint-ids vpce-xxxxxxxx` | Delete an endpoint |
| `aws ec2 describe-vpc-endpoint-services` | List available endpoint services |

```bash
# Gateway endpoint (S3)
aws ec2 create-vpc-endpoint \
  --vpc-id vpc-xxx \
  --service-name com.amazonaws.us-east-1.s3 \
  --route-table-ids rtb-xxx

# Interface endpoint (SQS)
aws ec2 create-vpc-endpoint \
  --vpc-id vpc-xxx \
  --service-name com.amazonaws.us-east-1.sqs \
  --vpc-endpoint-type Interface \
  --subnet-ids subnet-xxx \
  --security-group-ids sg-xxx
```

---

## Network ACLs

| Command | Description |
|---|---|
| `aws ec2 describe-network-acls` | List all NACLs |
| `aws ec2 create-network-acl --vpc-id vpc-xxx` | Create a NACL |
| `aws ec2 create-network-acl-entry --network-acl-id acl-xxx --rule-number 100 --protocol tcp --port-range From=80,To=80 --rule-action allow --egress` | Add outbound rule |
| `aws ec2 create-network-acl-entry --network-acl-id acl-xxx --rule-number 100 --protocol tcp --port-range From=80,To=80 --rule-action allow` | Add inbound rule |
| `aws ec2 delete-network-acl-entry --network-acl-id acl-xxx --rule-number 100 --egress` | Delete outbound rule |
| `aws ec2 replace-network-acl-subnet --network-acl-id acl-xxx --subnet-id subnet-xxx` | Associate NACL with subnet |

```bash
# Create NACL
aws ec2 create-network-acl --vpc-id vpc-xxx

# Allow HTTP inbound
aws ec2 create-network-acl-entry \
  --network-acl-id acl-xxx \
  --rule-number 100 \
  --protocol tcp \
  --port-range From=80,To=80 \
  --rule-action allow

# Allow all outbound
aws ec2 create-network-acl-entry \
  --network-acl-id acl-xxx \
  --rule-number 100 \
  --protocol -1 \
  --rule-action allow \
  --egress
```

---

## Flow Logs

| Command | Description |
|---|---|
| `aws ec2 describe-flow-logs` | List flow logs |
| `aws ec2 create-flow-logs --resource-type VPC --resource-ids vpc-xxx --traffic-type ALL --log-destination-type cloud-watch-logs --log-group-name /vpc/flowlogs --deliver-logs-permission-arn arn:aws:iam::ACCOUNT:role/FlowLogRole` | Create flow log to CloudWatch |
| `aws ec2 create-flow-logs --resource-type VPC --resource-ids vpc-xxx --traffic-type REJECT --log-destination-type s3 --log-destination arn:aws:s3:::my-bucket/flowlogs/` | Create flow log to S3 |
| `aws ec2 delete-flow-logs --flow-log-ids fl-xxxxxxxx` | Delete flow logs |

```bash
# Flow log to CloudWatch
aws ec2 create-flow-logs \
  --resource-type VPC \
  --resource-ids vpc-xxx \
  --traffic-type ALL \
  --log-destination-type cloud-watch-logs \
  --log-group-name /vpc/flowlogs \
  --deliver-logs-permission-arn arn:aws:iam::ACCOUNT:role/FlowLogRole
```

---

## Egress-Only Internet Gateway (IPv6)

| Command | Description |
|---|---|
| `aws ec2 create-egress-only-internet-gateway --vpc-id vpc-xxx` | Create egress-only IGW |
| `aws ec2 describe-egress-only-internet-gateways` | List egress-only IGWs |
| `aws ec2 delete-egress-only-internet-gateway --egress-only-internet-gateway-id eigw-xxx` | Delete egress-only IGW |

```bash
aws ec2 create-egress-only-internet-gateway --vpc-id vpc-xxx
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
