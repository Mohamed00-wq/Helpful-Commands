# Exports & Outputs

> Exports & Outputs. Part of the [CloudFormation](../) cheatsheet.

## To list all exports

The following example lists all exports.

```bash
aws cloudformation list-exports
```

## To find stacks using an export

The following example finds stacks using an export.

```bash
aws cloudformation list-imports --export-name my-export
```
