# Route Tables

> Route Tables. Part of the [VPC](../VPC.md) cheatsheet.

## To list route tables

The following example lists route tables.

```bash
aws ec2 describe-route-tables
```

## To create a route table

The following example creates a route table.

```bash
aws ec2 create-route-table --vpc-id vpc-xxxxxxxx
```

## To add route to IGW

The following example adds route to IGW.

```bash
aws ec2 create-route
    --route-table-id rtb-xxxxxxxx
    --destination-cidr-block 0.0.0.0/0
    --gateway-id igw-xxxxxxxx
```

## To add route to NAT

The following example adds route to NAT.

```bash
aws ec2 create-route
    --route-table-id rtb-xxxxxxxx
    --destination-cidr-block 0.0.0.0/0
    --nat-gateway-id nat-xxxxxxxx
```

## To delete a route

The following example deletes a route.

```bash
aws ec2 delete-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0
```

## To associate with subnet

The following example associates with subnet.

```bash
aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```

## To disassociate route table

The following example disassociates route table.

```bash
aws ec2 disassociate-route-table --association-id rtbassoc-xxxxxxxx
```
