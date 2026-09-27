# VPC Peering

> VPC Peering. Part of the [VPC](../VPC.md) cheatsheet.

## To list peering connections

The following example lists peering connections.

```bash
aws ec2 describe-vpc-peering-connections
```

## To create peering request

The following example creates peering request.

```bash
aws ec2 create-vpc-peering-connection --vpc-id vpc-xxx --peer-vpc-id vpc-yyy
```

## To accept peering request

The following example accepts peering request.

```bash
aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx
```

## To reject peering

The following example rejects peering.

```bash
aws ec2 reject-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx
```

## To delete peering

The following example deletes peering.

```bash
aws ec2 delete-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx
```

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
