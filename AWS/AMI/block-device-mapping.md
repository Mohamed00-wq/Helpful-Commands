# Block Device Mapping

> Block Device Mapping. Part of the [AMI](../) cheatsheet.

## To view AMI block device mappings

The following example views AMI block device mappings.

```bash
aws ec2 describe-image-attribute --image-id ami-xxxxxxxx --attribute blockDeviceMapping
```

## To modify AMI block device mapping

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
