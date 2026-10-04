# Describe / Search

> Describe / Search. Part of the [AMI](../) cheatsheet.

## To list your custom AMIs

The following example lists your custom AMIs.

```bash
aws ec2 describe-images --owners self
```

## To list AWS-owned AMIs

The following example lists AWS-owned AMIs.

```bash
aws ec2 describe-images --owners amazon
```

## To search for Amazon Linux 2 AMIs

The following example searches for Amazon Linux 2 AMIs.

```bash
aws ec2 describe-images --filters "Name=name,Values=amzn2-ami-hvm-*"
```

## To filter by architecture

The following example filters by architecture.

```bash
aws ec2 describe-images --filters "Name=architecture,Values=x86_64"
```

## To filter by virtualization type

The following example filters by virtualization type.

```bash
aws ec2 describe-images --filters "Name=virtualization-type,Values=hvm"
```

## To get details for a specific AMI

The following example gets details for a specific AMI.

```bash
aws ec2 describe-images --image-ids ami-xxxxxxxx
```
