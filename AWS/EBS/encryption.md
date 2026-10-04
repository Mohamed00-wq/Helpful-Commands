# Encryption

> Encryption. Part of the [EBS](../) cheatsheet.

## To enable default EBS encryption for the region

The following example enables default EBS encryption for the region.

```bash
aws ec2 enable-ebs-encryption-by-default
```

## To disable default EBS encryption

The following example disables default EBS encryption.

```bash
aws ec2 disable-ebs-encryption-by-default
```

## To check if default EBS encryption is enabled

The following example checks if default EBS encryption is enabled.

```bash
aws ec2 describe-ebs-encryption-by-default
```

## To list KMS keys (for custom encryption)

The following example lists KMS keys (for custom encryption).

```bash
aws ec2 describe-kms-aliases
```
