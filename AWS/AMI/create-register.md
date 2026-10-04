# Create / Register

> Create / Register. Part of the [AMI](../) cheatsheet.

## To create an AMI from a running instance (no reboot)

The following example creates an AMI from a running instance (no reboot).

```bash
aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami" --no-reboot
```

## To create an AMI from an instance (reboots)

The following example creates an AMI from an instance (reboots).

```bash
aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami"
```

## To register a new AMI

The following example registers a new AMI.

```bash
aws ec2 register-image
    --name "custom"
    --architecture x86_64
    --root-device-name /dev/xvda
    --virtualization-type hvm
```
