# EC2 (Elastic Compute Cloud)

> Commands for instances, key pairs, security groups, Elastic IPs, tags, and placement groups.

## Instances

### To list all EC2 instances and their details

### To get details for a specific instance

The following example gets details for a specific instance.

```bash
aws ec2 describe-instances --instance-ids i-xxxxxxxx
```

### To list only running instances

The following example lists only running instances.

```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
```

### To list instances by tag

The following example lists instances by tag.

```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=web-server"
```

### To launch a new instance

The following example launches a new instance.

```bash
aws ec2 run-instances
    --image-id ami-xxxxxxxx
    --instance-type t3.micro
    --key-name my-key
    --security-group-ids sg-xxxxxxxx
    --subnet-id subnet-xxxxxxxx
```

### To start a stopped instance

The following example starts a stopped instance.

```bash
aws ec2 start-instances --instance-ids i-xxxxxxxx
```

### To stop a running instance

The following example stops a running instance.

```bash
aws ec2 stop-instances --instance-ids i-xxxxxxxx
```

### To terminate (delete) an instance

The following example terminates (delete) an instance.

```bash
aws ec2 terminate-instances --instance-ids i-xxxxxxxx
```

### To reboot an instance

The following example reboots an instance.

```bash
aws ec2 reboot-instances --instance-ids i-xxxxxxxx
```

### To check instance status

The following example checks instance status.

```bash
aws ec2 describe-instance-status --instance-ids i-xxxxxxxx
```

### To wait until instance is running

The following example waits until instance is running.

```bash
aws ec2 wait instance-running --instance-ids i-xxxxxxxx
```

## Instance Types

### To list all available instance types

### To filter instance types by name

The following example filters instance types by name.

```bash
aws ec2 describe-instance-types --filters "Name=instance-type,Values=t3.*"
```

### To list instance type offerings per AZ

The following example lists instance type offerings per AZ.

```bash
aws ec2 describe-instance-type-offerings --location-type availability-zone --region us-east-1
```

## Key Pairs

### To list all key pairs

The following example lists all key pairs.

```bash
aws ec2 describe-key-pairs
```

### To create a new key pair

The following example creates a new key pair.

```bash
aws ec2 create-key-pair --key-name my-key
```

### To delete a key pair

The following example deletes a key pair.

```bash
aws ec2 delete-key-pair --key-name my-key
```

### To import a public key

The following example imports a public key.

```bash
aws ec2 import-key-pair --key-name my-key --public-key-material fileb://~/.ssh/id_rsa.pub
```

## Security Groups

### To list all security groups

### To get details for a specific SG

The following example gets details for a specific SG.

```bash
aws ec2 describe-security-groups --group-ids sg-xxxxxxxx
```

### To create a new security group

The following example creates a new security group.

```bash
aws ec2 create-security-group --group-name my-sg --description "My SG" --vpc-id vpc-xxxxxxxx
```

### To delete a security group

The following example deletes a security group.

```bash
aws ec2 delete-security-group --group-id sg-xxxxxxxx
```

### To add an inbound rule

The following example adds an inbound rule.

```bash
aws ec2 authorize-security-group-ingress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 80
    --cidr 0.0.0.0/0
```

### To add an outbound rule

The following example adds an outbound rule.

```bash
aws ec2 authorize-security-group-egress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 443
    --cidr 0.0.0.0/0
```

### To remove an inbound rule

The following example removes an inbound rule.

```bash
aws ec2 revoke-security-group-ingress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 80
    --cidr 0.0.0.0/0
```

### To remove an outbound rule

The following example removes an outbound rule.

```bash
aws ec2 revoke-security-group-egress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 443
    --cidr 0.0.0.0/0
```

## Elastic IPs

### To list all Elastic IPs

The following example lists all Elastic IPs.

```bash
aws ec2 describe-addresses
```

### To allocate a new Elastic IP

The following example allocates a new Elastic IP.

```bash
aws ec2 allocate-address --domain vpc
```

### To associate an EIP with an instance

The following example associates an EIP with an instance.

```bash
aws ec2 associate-address --instance-id i-xxxxxxxx --allocation-id eipalloc-xxxxxxxx
```

### To disassociate an EIP

The following example disassociates an EIP.

```bash
aws ec2 disassociate-address --association-id eipassoc-xxxxxxxx
```

### To release an Elastic IP

The following example releases an Elastic IP.

```bash
aws ec2 release-address --allocation-id eipalloc-xxxxxxxx
```

## VPC, Subnets & Route Tables

### To list VPCs in the account

The following example lists VPCs in the account.

```bash
aws ec2 describe-vpcs
```

### To list subnets

The following example lists subnets.

```bash
aws ec2 describe-subnets
```

### To list route tables

The following example lists route tables.

```bash
aws ec2 describe-route-tables
```

### To list NAT gateways

The following example lists NAT gateways.

```bash
aws ec2 describe-nat-gateways
```

### To list internet gateways

The following example lists internet gateways.

```bash
aws ec2 describe-internet-gateways
```

### To list network ACLs

The following example lists network ACLs.

```bash
aws ec2 describe-network-acls
```

## Tags

### To list all tags

The following example lists all tags.

```bash
aws ec2 describe-tags
```

### To add a tag to a resource

The following example adds a tag to a resource.

```bash
aws ec2 create-tags --resources i-xxxxxxxx --tags Key=Name,Value=web-server
```

### To remove a tag from a resource

The following example removes a tag from a resource.

```bash
aws ec2 delete-tags --resources i-xxxxxxxx --tags Key=Name
```

## Placement Groups

### To list placement groups

The following example lists placement groups.

```bash
aws ec2 describe-placement-groups
```

### To create a cluster placement group

The following example creates a cluster placement group.

```bash
aws ec2 create-placement-group --group-name my-pg --strategy cluster
```

### To delete a placement group

The following example deletes a placement group.

```bash
aws ec2 delete-placement-group --group-name my-pg
```
