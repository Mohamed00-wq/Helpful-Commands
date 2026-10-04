# Key Pairs

> Key Pairs. Part of the [EC2](../) cheatsheet.

## To list all key pairs

The following example lists all key pairs.

```bash
aws ec2 describe-key-pairs
```

## To create a new key pair

The following example creates a new key pair.

```bash
aws ec2 create-key-pair --key-name my-key
```

## To delete a key pair

The following example deletes a key pair.

```bash
aws ec2 delete-key-pair --key-name my-key
```

## To import a public key

The following example imports a public key.

```bash
aws ec2 import-key-pair --key-name my-key --public-key-material fileb://~/.ssh/id_rsa.pub
```
