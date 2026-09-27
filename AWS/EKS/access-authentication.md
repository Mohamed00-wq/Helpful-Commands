# Access & Authentication

> Access & Authentication. Part of the [EKS](../EKS.md) cheatsheet.

## To update kubeconfig for the cluster

The following example updates kubeconfig for the cluster.

```bash
aws eks update-kubeconfig --name my-cluster
```

## To update kubeconfig for specific region

The following example updates kubeconfig for specific region.

```bash
aws eks update-kubeconfig --name my-cluster --region us-west-2
```

## To list access entries

The following example lists access entries.

```bash
aws eks list-access-entries --cluster-name my-cluster
```

## To add access entry

The following example adds access entry.

```bash
aws eks create-access-entry
    --cluster-name my-cluster
    --principal-arn arn:aws:iam::ACCOUNT:user/alice
```

## To remove access entry

The following example removes access entry.

```bash
aws eks delete-access-entry
    --cluster-name my-cluster
    --principal-arn arn:aws:iam::ACCOUNT:user/alice
```

## To attach access policy

The following example attaches access policy.

```bash
aws eks associate-access-policy
    --cluster-name my-cluster
    --principal-arn arn:aws:iam::ACCOUNT:user/alice
    --access-policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy
```
