# Subnets

> Subnets. Part of the [VPC](../) cheatsheet.

## To list all subnets

## To get subnet details

The following example gets subnet details.

```bash
aws ec2 describe-subnets --subnet-ids subnet-xxxxxxxx
```

## To create a subnet

The following example creates a subnet.

```bash
aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone us-east-1a
```

## To delete a subnet

The following example deletes a subnet.

```bash
aws ec2 delete-subnet --subnet-id subnet-xxxxxxxx
```

## To make subnet public

The following example makes subnet public.

```bash
aws ec2 modify-subnet-attribute --subnet-id subnet-xxxxxxxx --map-public-ip-on-launch
```

## To make subnet private

The following example makes subnet private.

```bash
aws ec2 modify-subnet-attribute --subnet-id subnet-xxxxxxxx --no-map-public-ip-on-launch
```

## To associate route table

The following example associates route table.

```bash
aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```
