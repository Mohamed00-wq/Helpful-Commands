# RDS (Relational Database Service)

> Commands for instances, Aurora, read replicas, snapshots, and parameter groups.

## DB Instances

### To list all RDS instances

### To get details for a specific instance

The following example gets details for a specific instance.

```bash
aws rds describe-db-instances --db-instance-identifier mydb
```

### To create a MySQL instance

The following example creates a MySQL instance.

```bash
aws rds create-db-instance
    --db-instance-identifier mydb
    --db-instance-class db.t3.micro
    --engine mysql
    --master-username admin
    --master-user-password P@ssw0rd!
    --allocated-storage 20
```

### To create a PostgreSQL instance

The following example creates a PostgreSQL instance.

```bash
aws rds create-db-instance
    --db-instance-identifier mydb
    --db-instance-class db.t3.micro
    --engine postgres
    --master-username admin
    --master-user-password P@ssw0rd!
    --allocated-storage 20
    --db-name mydb
```

### To delete without final snapshot

The following example deletes without final snapshot.

```bash
aws rds delete-db-instance --db-instance-identifier mydb --skip-final-snapshot
```

### To delete with final snapshot

The following example deletes with final snapshot.

```bash
aws rds delete-db-instance --db-instance-identifier mydb --final-db-snapshot-identifier mydb-final
```

### To stop an instance (non-Aurora)

The following example stops an instance (non-Aurora).

```bash
aws rds stop-db-instance --db-instance-identifier mydb
```

### To start a stopped instance

The following example starts a stopped instance.

```bash
aws rds start-db-instance --db-instance-identifier mydb
```

### To reboot an instance

The following example reboots an instance.

```bash
aws rds reboot-db-instance --db-instance-identifier mydb
```

### To view automated backups

The following example views automated backups.

```bash
aws rds describe-db-instance-automated-backups --db-instance-identifier mydb
```

## Aurora Clusters

### To list all Aurora clusters

### To get cluster details

The following example gets cluster details.

```bash
aws rds describe-db-clusters --db-cluster-identifier my-cluster
```

### To create Aurora MySQL cluster

The following example creates Aurora MySQL cluster.

```bash
aws rds create-db-cluster
    --db-cluster-identifier my-cluster
    --engine aurora-mysql
    --master-username admin
    --master-user-password P@ssw0rd!
```

### To create Aurora PostgreSQL cluster

The following example creates Aurora PostgreSQL cluster.

```bash
aws rds create-db-cluster
    --db-cluster-identifier my-cluster
    --engine aurora-postgresql
    --master-username admin
    --master-user-password P@ssw0rd!
```

### To delete cluster without snapshot

The following example deletes cluster without snapshot.

```bash
aws rds delete-db-cluster --db-cluster-identifier my-cluster --skip-final-snapshot
```

### To delete with final snapshot

The following example deletes with final snapshot.

```bash
aws rds delete-db-cluster
    --db-cluster-identifier my-cluster
    --final-db-snapshot-identifier my-cluster-final
```

### To add instance to cluster

The following example adds instance to cluster.

```bash
aws rds create-db-instance
    --db-cluster-identifier my-cluster
    --db-instance-identifier mydb-1
    --db-instance-class db.r5.large
    --engine aurora-mysql
```

### To get cluster endpoint

The following example gets cluster endpoint.

```bash
aws rds describe-db-clusters --db-cluster-identifier my-cluster --query "DBClusters[0].Endpoint"
```

## Read Replicas

### To create a read replica

The following example creates a read replica.

```bash
aws rds create-db-instance-read-replica
    --db-instance-identifier mydb-replica
    --source-db-instance-identifier mydb
```

### To create cross-region read replica

The following example creates cross-region read replica.

```bash
aws rds create-db-instance-read-replica
    --db-instance-identifier mydb-replica
    --source-db-instance-identifier mydb
    --source-region us-east-1
    --region us-west-2
```

### To delete a read replica

The following example deletes a read replica.

```bash
aws rds delete-db-instance --db-instance-identifier mydb-replica --skip-final-snapshot
```

### To promote read replica to standalone

The following example promotes read replica to standalone.

```bash
aws rds promote-read-replica --db-instance-identifier mydb-replica
```

## Snapshots

### To create a manual snapshot

The following example creates a manual snapshot.

```bash
aws rds create-db-snapshot --db-snapshot-identifier my-snap --db-instance-identifier mydb
```

### To get snapshot details

The following example gets snapshot details.

```bash
aws rds describe-db-snapshots --db-snapshot-identifier my-snap
```

### To list snapshots for an instance

The following example lists snapshots for an instance.

```bash
aws rds describe-db-snapshots --db-instance-identifier mydb
```

### To delete a manual snapshot

The following example deletes a manual snapshot.

```bash
aws rds delete-db-snapshot --db-snapshot-identifier my-snap
```

### To copy snapshot to another region

The following example copies snapshot to another region.

```bash
aws rds copy-db-snapshot
    --source-db-snapshot-identifier my-snap
    --target-db-snapshot-identifier my-snap-copy
    --source-region us-east-1
    --region us-west-2
```

### To restore from snapshot

The following example restores from snapshot.

```bash
aws rds restore-db-instance-from-db-snapshot
    --db-instance-identifier mydb-restored
    --db-snapshot-identifier my-snap
```

### To restore to point in time

The following example restores to point in time.

```bash
aws rds restore-db-instance-to-point-in-time
    --db-instance-identifier mydb-restored
    --source-db-instance-identifier mydb
    --restore-time "2026-01-15T12:00:00Z"
```

### To view snapshot sharing

The following example views snapshot sharing.

```bash
aws rds describe-db-snapshot-attributes --db-snapshot-identifier my-snap
```

### To share snapshot publicly

The following example shares snapshot publicly.

```bash
aws rds modify-db-snapshot-attribute
    --db-snapshot-identifier my-snap
    --attribute-name restore
    --values-to-add all
```

## Parameter Groups

### To list parameter groups

The following example lists parameter groups.

```bash
aws rds describe-db-parameter-groups
```

### To list parameters in a group

The following example lists parameters in a group.

```bash
aws rds describe-db-parameters --db-parameter-group-name my-pg
```

### To create a parameter group

The following example creates a parameter group.

```bash
aws rds create-db-parameter-group
    --db-parameter-group-name my-pg
    --db-parameter-group-family mysql8.0
    --description "My parameter group"
```

### To delete a parameter group

The following example deletes a parameter group.

```bash
aws rds delete-db-parameter-group --db-parameter-group-name my-pg
```

### To modify a parameter

The following example modifies a parameter.

```bash
aws rds modify-db-parameter-group
    --db-parameter-group-name my-pg
    --parameters "ParameterName=max_connections,ParameterValue=200,ApplyMethod=pending-reboot"
```

### To reset a parameter to default

The following example resets a parameter to default.

```bash
aws rds reset-db-parameter-group
    --db-parameter-group-name my-pg
    --parameters "ParameterName=max_connections,ApplyMethod=pending-reboot"
```

## Option Groups

### To list option groups

The following example lists option groups.

```bash
aws rds describe-option-groups
```

### To create an option group

The following example creates an option group.

```bash
aws rds create-option-group
    --option-group-name my-og
    --engine-name mysql
    --major-engine-version 8.0
    --description "My option group"
```

### To delete an option group

The following example deletes an option group.

```bash
aws rds delete-option-group --option-group-name my-og
```

### To add an option

The following example adds an option.

```bash
aws rds add-option-to-option-group
    --option-group-name my-og
    --option-name MARIADB_AUDIT_PLUGIN
    --apply-immediately
```

### To remove an option

The following example removes an option.

```bash
aws rds remove-option-from-option-group
    --option-group-name my-og
    --option-name MARIADB_AUDIT_PLUGIN
    --apply-immediately
```

## DB Subnet Groups

### To list DB subnet groups

The following example lists DB subnet groups.

```bash
aws rds describe-db-subnet-groups
```

### To create a subnet group

The following example creates a subnet group.

```bash
aws rds create-db-subnet-group
    --db-subnet-group-name my-sg
    --subnet-ids "subnet-xxxxxxxx subnet-yyyyyyyy"
    --description "My DB subnet group"
```

### To delete a subnet group

The following example deletes a subnet group.

```bash
aws rds delete-db-subnet-group --db-subnet-group-name my-sg
```

## Events & Notifications

### To list RDS events

The following example lists RDS events.

```bash
aws rds describe-events --source-type db-instance --start-time 2026-01-15T00:00:00Z
```

### To list event subscriptions

The following example lists event subscriptions.

```bash
aws rds describe-event-subscriptions
```

### To create event subscription

The following example creates event subscription.

```bash
aws rds create-event-subscription
    --subscription-name my-sub
    --sns-topic-arn arn:aws:sns:us-east-1:123456789012:my-topic
    --source-type db-instance
```

### To delete event subscription

The following example deletes event subscription.

```bash
aws rds delete-event-subscription --subscription-name my-sub
```

## Global Database (Aurora)

### To list Aurora Global Databases

The following example lists Aurora Global Databases.

```bash
aws rds describe-global-clusters
```

### To create a global cluster

The following example creates a global cluster.

```bash
aws rds create-global-cluster --global-cluster-identifier my-global --engine aurora-mysql
```

### To add primary cluster

The following example adds primary cluster.

```bash
aws rds add-source-cluster-to-global-cluster
    --global-cluster-identifier my-global
    --source-db-cluster-arn arn:aws:rds:us-east-1:ACCOUNT:cluster:my-cluster
```

### To delete global cluster

The following example deletes global cluster.

```bash
aws rds delete-global-cluster --global-cluster-identifier my-global
```
