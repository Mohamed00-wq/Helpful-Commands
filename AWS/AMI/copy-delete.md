# Copy / Delete

> Copy / Delete. Part of the [AMI](../AMI.md) cheatsheet.

## To copy an AMI to another region

The following example copies an AMI to another region.

```bash
aws ec2 copy-image
    --source-image-id ami-xxxxxxxx
    --source-region us-east-1
    --destination-region us-west-2
```

## To copy an AMI with encryption

The following example copies an AMI with encryption.

```bash
aws ec2 copy-image --source-image-id ami-xxxxxxxx --source-region us-east-1 --encrypted
```

## To delete (deregister) an AMI

The following example deletes (deregister) an AMI.

```bash
aws ec2 deregister-image --image-id ami-xxxxxxxx
```
