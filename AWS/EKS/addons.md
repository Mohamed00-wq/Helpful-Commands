# Addons

> Addons. Part of the [EKS](../EKS.md) cheatsheet.

## To list installed addons

The following example lists installed addons.

```bash
aws eks list-addons --cluster-name my-cluster
```

## To get addon details

The following example gets addon details.

```bash
aws eks describe-addon --cluster-name my-cluster --addon-name vpc-cni
```

## To install an addon

The following example installs an addon.

```bash
aws eks create-addon --cluster-name my-cluster --addon-name vpc-cni --resolve-conflicts OVERWRITE
```

## To update an addon

The following example updates an addon.

```bash
aws eks update-addon --cluster-name my-cluster --addon-name vpc-cni --addon-version v1.15.1
```

## To delete an addon

The following example deletes an addon.

```bash
aws eks delete-addon --cluster-name my-cluster --addon-name vpc-cni
```

```bash
# Install VPC CNI
aws eks create-addon \
    --cluster-name my-cluster \
    --addon-name vpc-cni \
    --resolve-conflicts OVERWRITE

# Install CoreDNS
aws eks create-addon \
    --cluster-name my-cluster \
    --addon-name coredns \
    --resolve-conflicts OVERWRITE
```
