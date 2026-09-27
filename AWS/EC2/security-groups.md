# Security Groups

> Security Groups. Part of the [EC2](../EC2.md) cheatsheet.

## To list all security groups

## To get details for a specific SG

The following example gets details for a specific SG.

```bash
aws ec2 describe-security-groups --group-ids sg-xxxxxxxx
```

## To create a new security group

The following example creates a new security group.

```bash
aws ec2 create-security-group --group-name my-sg --description "My SG" --vpc-id vpc-xxxxxxxx
```

## To delete a security group

The following example deletes a security group.

```bash
aws ec2 delete-security-group --group-id sg-xxxxxxxx
```

## To add an inbound rule

The following example adds an inbound rule.

```bash
aws ec2 authorize-security-group-ingress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 80
    --cidr 0.0.0.0/0
```

## To add an outbound rule

The following example adds an outbound rule.

```bash
aws ec2 authorize-security-group-egress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 443
    --cidr 0.0.0.0/0
```

## To remove an inbound rule

The following example removes an inbound rule.

```bash
aws ec2 revoke-security-group-ingress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 80
    --cidr 0.0.0.0/0
```

## To remove an outbound rule

The following example removes an outbound rule.

```bash
aws ec2 revoke-security-group-egress
    --group-id sg-xxxxxxxx
    --protocol tcp
    --port 443
    --cidr 0.0.0.0/0
```
