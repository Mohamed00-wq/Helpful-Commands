# Elastic IPs

> Elastic IPs. Part of the [EC2](../) cheatsheet.

## To list all Elastic IPs

The following example lists all Elastic IPs.

```bash
aws ec2 describe-addresses
```

## To allocate a new Elastic IP

The following example allocates a new Elastic IP.

```bash
aws ec2 allocate-address --domain vpc
```

## To associate an EIP with an instance

The following example associates an EIP with an instance.

```bash
aws ec2 associate-address --instance-id i-xxxxxxxx --allocation-id eipalloc-xxxxxxxx
```

## To disassociate an EIP

The following example disassociates an EIP.

```bash
aws ec2 disassociate-address --association-id eipassoc-xxxxxxxx
```

## To release an Elastic IP

The following example releases an Elastic IP.

```bash
aws ec2 release-address --allocation-id eipalloc-xxxxxxxx
```
