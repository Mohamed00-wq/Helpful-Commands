# VPC Operations

> VPC Operations. Part of the [VPC](../VPC.md) cheatsheet.

## To list all VPCs

## To get VPC details

The following example gets VPC details.

```bash
aws ec2 describe-vpcs --vpc-ids vpc-xxxxxxxx
```

## To create a VPC

The following example creates a VPC.

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

## To delete a VPC

The following example deletes a VPC.

```bash
aws ec2 delete-vpc --vpc-id vpc-xxxxxxxx
```

## To enable DNS support

The following example enables DNS support.

```bash
aws ec2 modify-vpc-attribute --vpc-id vpc-xxxxxxxx --enable-dns-support
```

## To enable DNS hostnames

The following example enables DNS hostnames.

```bash
aws ec2 modify-vpc-attribute --vpc-id vpc-xxxxxxxx --enable-dns-hostnames
```

## To check DNS support

The following example checks DNS support.

```bash
aws ec2 describe-vpc-attribute --vpc-id vpc-xxxxxxxx --attribute enableDnsSupport
```
