# Global Database (Aurora)

> Global Database (Aurora). Part of the [RDS](../) cheatsheet.

## To list Aurora Global Databases

The following example lists Aurora Global Databases.

```bash
aws rds describe-global-clusters
```

## To create a global cluster

The following example creates a global cluster.

```bash
aws rds create-global-cluster --global-cluster-identifier my-global --engine aurora-mysql
```

## To add primary cluster

The following example adds primary cluster.

```bash
aws rds add-source-cluster-to-global-cluster
    --global-cluster-identifier my-global
    --source-db-cluster-arn arn:aws:rds:us-east-1:ACCOUNT:cluster:my-cluster
```

## To delete global cluster

The following example deletes global cluster.

```bash
aws rds delete-global-cluster --global-cluster-identifier my-global
```
