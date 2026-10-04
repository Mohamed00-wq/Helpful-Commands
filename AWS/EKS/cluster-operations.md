# Cluster Operations

> Cluster Operations. Part of the [EKS](../) cheatsheet.

## To list all EKS clusters

The following example lists all EKS clusters.

```bash
aws eks list-clusters
```

## To get cluster details

The following example gets cluster details.

```bash
aws eks describe-cluster --name my-cluster
```

## To create a cluster

The following example creates a cluster.

```bash
aws eks create-cluster
    --name my-cluster
    --role-arn arn:aws:iam::ACCOUNT:role/EKSClusterRole
    --resources-vpc-config subnetIds=subnet-xxx,subnet-yyy
```

## To delete a cluster

The following example deletes a cluster.

```bash
aws eks delete-cluster --name my-cluster
```

## To enable cluster logging

The following example enables cluster logging.

```bash
aws eks update-cluster-config
    --name my-cluster
    --logging '{"clusterLogging":[{"types":["api","audit"],"enabled":true}]}'
```

## To get OIDC issuer URL

The following example gets OIDC issuer URL.

```bash
aws eks describe-cluster --name my-cluster --query "cluster.identity.oidc.issuer"
```
