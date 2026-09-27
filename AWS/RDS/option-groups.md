# Option Groups

> Option Groups. Part of the [RDS](../RDS.md) cheatsheet.

## To list option groups

The following example lists option groups.

```bash
aws rds describe-option-groups
```

## To create an option group

The following example creates an option group.

```bash
aws rds create-option-group
    --option-group-name my-og
    --engine-name mysql
    --major-engine-version 8.0
    --description "My option group"
```

## To delete an option group

The following example deletes an option group.

```bash
aws rds delete-option-group --option-group-name my-og
```

## To add an option

The following example adds an option.

```bash
aws rds add-option-to-option-group
    --option-group-name my-og
    --option-name MARIADB_AUDIT_PLUGIN
    --apply-immediately
```

## To remove an option

The following example removes an option.

```bash
aws rds remove-option-from-option-group
    --option-group-name my-og
    --option-name MARIADB_AUDIT_PLUGIN
    --apply-immediately
```
