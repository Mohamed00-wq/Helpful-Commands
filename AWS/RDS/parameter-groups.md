# Parameter Groups

> Parameter Groups. Part of the [RDS](../RDS.md) cheatsheet.

## To list parameter groups

The following example lists parameter groups.

```bash
aws rds describe-db-parameter-groups
```

## To list parameters in a group

The following example lists parameters in a group.

```bash
aws rds describe-db-parameters --db-parameter-group-name my-pg
```

## To create a parameter group

The following example creates a parameter group.

```bash
aws rds create-db-parameter-group
    --db-parameter-group-name my-pg
    --db-parameter-group-family mysql8.0
    --description "My parameter group"
```

## To delete a parameter group

The following example deletes a parameter group.

```bash
aws rds delete-db-parameter-group --db-parameter-group-name my-pg
```

## To modify a parameter

The following example modifies a parameter.

```bash
aws rds modify-db-parameter-group
    --db-parameter-group-name my-pg
    --parameters "ParameterName=max_connections,ParameterValue=200,ApplyMethod=pending-reboot"
```

## To reset a parameter to default

The following example resets a parameter to default.

```bash
aws rds reset-db-parameter-group
    --db-parameter-group-name my-pg
    --parameters "ParameterName=max_connections,ApplyMethod=pending-reboot"
```
