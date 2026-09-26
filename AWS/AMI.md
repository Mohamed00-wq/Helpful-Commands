# AMI (Amazon Machine Images)

> Commands for creating, copying, sharing, and deregistering machine images.

## Describe / Search

### To list your custom AMIs

The following example lists your custom AMIs.

```bash
aws ec2 describe-images --owners self
```

### To list AWS-owned AMIs

The following example lists AWS-owned AMIs.

```bash
aws ec2 describe-images --owners amazon
```

### To search for Amazon Linux 2 AMIs

The following example searches for Amazon Linux 2 AMIs.

```bash
aws ec2 describe-images --filters "Name=name,Values=amzn2-ami-hvm-*"
```

### To filter by architecture

The following example filters by architecture.

```bash
aws ec2 describe-images --filters "Name=architecture,Values=x86_64"
```

### To filter by virtualization type

The following example filters by virtualization type.

```bash
aws ec2 describe-images --filters "Name=virtualization-type,Values=hvm"
```

### To get details for a specific AMI

The following example gets details for a specific AMI.

```bash
aws ec2 describe-images --image-ids ami-xxxxxxxx
```

## Create / Register

### To create an AMI from a running instance (no reboot)

The following example creates an AMI from a running instance (no reboot).

```bash
aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami" --no-reboot
```

### To create an AMI from an instance (reboots)

The following example creates an AMI from an instance (reboots).

```bash
aws ec2 create-image --instance-id i-xxxxxxxx --name "my-ami"
```

### To register a new AMI

The following example registers a new AMI.

```bash
aws ec2 register-image
    --name "custom"
    --architecture x86_64
    --root-device-name /dev/xvda
    --virtualization-type hvm
```

## Copy / Delete

### To copy an AMI to another region

The following example copies an AMI to another region.

```bash
aws ec2 copy-image
    --source-image-id ami-xxxxxxxx
    --source-region us-east-1
    --destination-region us-west-2
```

### To copy an AMI with encryption

The following example copies an AMI with encryption.

```bash
aws ec2 copy-image --source-image-id ami-xxxxxxxx --source-region us-east-1 --encrypted
```

### To delete (deregister) an AMI

The following example deletes (deregister) an AMI.

```bash
aws ec2 deregister-image --image-id ami-xxxxxxxx
```

## Sharing / Permissions

### To view AMI sharing permissions

The following example views AMI sharing permissions.

```bash
aws ec2 describe-image-attribute --image-id ami-xxxxxxxx --attribute launchPermission
```

### To share an AMI with another account

The following example shares an AMI with another account.

```bash
aws ec2 modify-image-attribute
    --image-id ami-xxxxxxxx
    --launch-permissions "Add=[{UserId=123456789012}]"
```

### To stop sharing an AMI with an account

The following example stops sharing an AMI with an account.

```bash
aws ec2 modify-image-attribute
    --image-id ami-xxxxxxxx
    --launch-permissions "Remove=[{UserId=123456789012}]"
```

### To make an AMI public

The following example makes an AMI public.

```bash
aws ec2 modify-image-attribute --image-id ami-xxxxxxxx --launch-permissions "Add=[{Group=all}]"
```

## Block Device Mapping

### To view AMI block device mappings

The following example views AMI block device mappings.

```bash
aws ec2 describe-image-attribute --image-id ami-xxxxxxxx --attribute blockDeviceMapping
```

### To modify AMI block device mapping

The following example modifies AMI block device mapping.

```bash
aws ec2 modify-image-attribute
    --image-id ami-xxxxxxxx
    --block-device-mappings "[{\" DeviceName\":\"/dev/xvda\",\"Ebs\":{\"VolumeSize\":50,\"VolumeType\":\"gp3\"}}]"
```

```bash
aws ec2 describe-image-attribute \
    --image-id ami-xxxxxxxx \
    --attribute blockDeviceMapping
aws ec2 modify-image-attribute \
    --image-id ami-xxxxxxxx \
    --block-device-mappings '[{"DeviceName":"/dev/xvda","Ebs":{"VolumeSize":50,"VolumeType":"gp3"}}]'
```
