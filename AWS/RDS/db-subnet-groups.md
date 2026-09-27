# DB Subnet Groups

> DB Subnet Groups. Part of the [RDS](../RDS.md) cheatsheet.

## To list DB subnet groups

The following example lists DB subnet groups.

```bash
aws rds describe-db-subnet-groups
```

## To create a subnet group

The following example creates a subnet group.

```bash
aws rds create-db-subnet-group
    --db-subnet-group-name my-sg
    --subnet-ids "subnet-xxxxxxxx subnet-yyyyyyyy"
    --description "My DB subnet group"
```

## To delete a subnet group

The following example deletes a subnet group.

```bash
aws rds delete-db-subnet-group --db-subnet-group-name my-sg
```
