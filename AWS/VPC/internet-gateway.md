# Internet Gateway

> Internet Gateway. Part of the [VPC](../) cheatsheet.

## To list IGWs

The following example lists IGWs.

```bash
aws ec2 describe-internet-gateways
```

## To create an IGW

The following example creates an IGW.

```bash
aws ec2 create-internet-gateway
```

## To attach IGW to VPC

The following example attaches IGW to VPC.

```bash
aws ec2 attach-internet-gateway --internet-gateway-id igw-xxxxxxxx --vpc-id vpc-xxxxxxxx
```

## To detach IGW

The following example detaches IGW.

```bash
aws ec2 detach-internet-gateway --internet-gateway-id igw-xxxxxxxx --vpc-id vpc-xxxxxxxx
```

## To delete IGW

The following example deletes IGW.

```bash
aws ec2 delete-internet-gateway --internet-gateway-id igw-xxxxxxxx
```
