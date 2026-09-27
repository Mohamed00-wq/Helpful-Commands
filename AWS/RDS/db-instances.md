# DB Instances

> DB Instances. Part of the [RDS](../RDS.md) cheatsheet.

## To list all RDS instances

## To get details for a specific instance

The following example gets details for a specific instance.

```bash
aws rds describe-db-instances --db-instance-identifier mydb
```

## To create a MySQL instance

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

## To create a PostgreSQL instance

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

## To delete without final snapshot

The following example deletes without final snapshot.

```bash
aws rds delete-db-instance --db-instance-identifier mydb --skip-final-snapshot
```

## To delete with final snapshot

The following example deletes with final snapshot.

```bash
aws rds delete-db-instance --db-instance-identifier mydb --final-db-snapshot-identifier mydb-final
```

## To stop an instance (non-Aurora)

The following example stops an instance (non-Aurora).

```bash
aws rds stop-db-instance --db-instance-identifier mydb
```

## To start a stopped instance

The following example starts a stopped instance.

```bash
aws rds start-db-instance --db-instance-identifier mydb
```

## To reboot an instance

The following example reboots an instance.

```bash
aws rds reboot-db-instance --db-instance-identifier mydb
```

## To view automated backups

The following example views automated backups.

```bash
aws rds describe-db-instance-automated-backups --db-instance-identifier mydb
```
