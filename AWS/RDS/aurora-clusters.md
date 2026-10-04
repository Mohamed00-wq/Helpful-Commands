# Aurora Clusters

> Aurora Clusters. Part of the [RDS](../) cheatsheet.

## To list all Aurora clusters

## To get cluster details

The following example gets cluster details.

```bash
aws rds describe-db-clusters --db-cluster-identifier my-cluster
```

## To create Aurora MySQL cluster

The following example creates Aurora MySQL cluster.

```bash
aws rds create-db-cluster
    --db-cluster-identifier my-cluster
    --engine aurora-mysql
    --master-username admin
    --master-user-password P@ssw0rd!
```

## To create Aurora PostgreSQL cluster

The following example creates Aurora PostgreSQL cluster.

```bash
aws rds create-db-cluster
    --db-cluster-identifier my-cluster
    --engine aurora-postgresql
    --master-username admin
    --master-user-password P@ssw0rd!
```

## To delete cluster without snapshot

The following example deletes cluster without snapshot.

```bash
aws rds delete-db-cluster --db-cluster-identifier my-cluster --skip-final-snapshot
```

## To delete with final snapshot

The following example deletes with final snapshot.

```bash
aws rds delete-db-cluster
    --db-cluster-identifier my-cluster
    --final-db-snapshot-identifier my-cluster-final
```

## To add instance to cluster

The following example adds instance to cluster.

```bash
aws rds create-db-instance
    --db-cluster-identifier my-cluster
    --db-instance-identifier mydb-1
    --db-instance-class db.r5.large
    --engine aurora-mysql
```

## To get cluster endpoint

The following example gets cluster endpoint.

```bash
aws rds describe-db-clusters --db-cluster-identifier my-cluster --query "DBClusters[0].Endpoint"
```
