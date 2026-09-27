# Egress-Only Internet Gateway (IPv6)

> Egress-Only Internet Gateway (IPv6). Part of the [VPC](../VPC.md) cheatsheet.

## To create egress-only IGW

The following example creates egress-only IGW.

```bash
aws ec2 create-egress-only-internet-gateway --vpc-id vpc-xxx
```

## To list egress-only IGWs

The following example lists egress-only IGWs.

```bash
aws ec2 describe-egress-only-internet-gateways
```

## To delete egress-only IGW

The following example deletes egress-only IGW.

```bash
aws ec2 delete-egress-only-internet-gateway --egress-only-internet-gateway-id eigw-xxx
```
