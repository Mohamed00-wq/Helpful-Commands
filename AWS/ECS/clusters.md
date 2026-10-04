# Clusters

> Clusters. Part of the [ECS](../) cheatsheet.

## To list all clusters

The following example lists all clusters.

```bash
aws ecs list-clusters
```

## To get cluster details

The following example gets cluster details.

```bash
aws ecs describe-clusters --clusters my-cluster
```

## To create a cluster

The following example creates a cluster.

```bash
aws ecs create-cluster --cluster-name my-cluster
```

## To create with capacity providers

The following example creates with capacity providers.

```bash
aws ecs create-cluster --cluster-name my-cluster --capacity-providers FARGATE FARGATE_SPOT
```

## To delete a cluster

The following example deletes a cluster.

```bash
aws ecs delete-cluster --cluster my-cluster
```
