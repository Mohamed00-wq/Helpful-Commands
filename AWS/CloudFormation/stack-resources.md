# Stack Resources

> Stack Resources. Part of the [CloudFormation](../) cheatsheet.

## To list all resources in a stack

The following example lists all resources in a stack.

```bash
aws cloudformation list-stack-resources --stack-name my-stack
```

## To get details for a specific resource

The following example gets details for a specific resource.

```bash
aws cloudformation describe-stack-resource --stack-name my-stack --logical-resource-id MyEC2Instance
```

## To find resource by physical ID

The following example finds resource by physical ID.

```bash
aws cloudformation describe-stack-resources --stack-name my-stack --physical-resource-id i-xxxxxxxx
```

## To continue rollback after failure

The following example continues rollback after failure.

```bash
aws cloudformation continue-update-rollback --stack-name my-stack
```
