# Instances

> Instances. Part of the [EC2](../) cheatsheet.

## To list all EC2 instances and their details

## To get details for a specific instance

The following example gets details for a specific instance.

```bash
aws ec2 describe-instances --instance-ids i-xxxxxxxx
```

## To list only running instances

The following example lists only running instances.

```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
```

## To list instances by tag

The following example lists instances by tag.

```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=web-server"
```

## To launch a new instance

The following example launches a new instance.

```bash
aws ec2 run-instances
    --image-id ami-xxxxxxxx
    --instance-type t3.micro
    --key-name my-key
    --security-group-ids sg-xxxxxxxx
    --subnet-id subnet-xxxxxxxx
```

## To start a stopped instance

The following example starts a stopped instance.

```bash
aws ec2 start-instances --instance-ids i-xxxxxxxx
```

## To stop a running instance

The following example stops a running instance.

```bash
aws ec2 stop-instances --instance-ids i-xxxxxxxx
```

## To terminate (delete) an instance

The following example terminates (delete) an instance.

```bash
aws ec2 terminate-instances --instance-ids i-xxxxxxxx
```

## To reboot an instance

The following example reboots an instance.

```bash
aws ec2 reboot-instances --instance-ids i-xxxxxxxx
```

## To check instance status

The following example checks instance status.

```bash
aws ec2 describe-instance-status --instance-ids i-xxxxxxxx
```

## To wait until instance is running

The following example waits until instance is running.

```bash
aws ec2 wait instance-running --instance-ids i-xxxxxxxx
```
