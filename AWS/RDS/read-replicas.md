# Read Replicas

> Read Replicas. Part of the [RDS](../RDS.md) cheatsheet.

## To create a read replica

The following example creates a read replica.

```bash
aws rds create-db-instance-read-replica
    --db-instance-identifier mydb-replica
    --source-db-instance-identifier mydb
```

## To create cross-region read replica

The following example creates cross-region read replica.

```bash
aws rds create-db-instance-read-replica
    --db-instance-identifier mydb-replica
    --source-db-instance-identifier mydb
    --source-region us-east-1
    --region us-west-2
```

## To delete a read replica

The following example deletes a read replica.

```bash
aws rds delete-db-instance --db-instance-identifier mydb-replica --skip-final-snapshot
```

## To promote read replica to standalone

The following example promotes read replica to standalone.

```bash
aws rds promote-read-replica --db-instance-identifier mydb-replica
```
