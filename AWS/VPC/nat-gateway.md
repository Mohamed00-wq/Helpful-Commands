# NAT Gateway

> NAT Gateway. Part of the [VPC](../) cheatsheet.

## To list NAT gateways

## To create a NAT gateway

The following example creates a NAT gateway.

```bash
aws ec2 create-nat-gateway --subnet-id subnet-xxxxxxxx --allocation-id eipalloc-xxxxxxxx
```

## To delete a NAT gateway

The following example deletes a NAT gateway.

```bash
aws ec2 delete-nat-gateway --nat-gateway-id nat-xxxxxxxx
```

## To list NAT gateways for a VPC

The following example lists NAT gateways for a VPC.

```bash
aws ec2 describe-nat-gateways --filter "Name=vpc-id,Values=vpc-xxx"
```

```bash
# Allocate EIP first
aws ec2 allocate-address --domain vpc

# Create NAT gateway
aws ec2 create-nat-gateway \
    --subnet-id subnet-xxx \
    --allocation-id eipalloc-xxx
```
