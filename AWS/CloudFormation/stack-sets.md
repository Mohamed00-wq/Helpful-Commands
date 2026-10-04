# Stack Sets

> Stack Sets. Part of the [CloudFormation](../) cheatsheet.

## To list all stack sets

The following example lists all stack sets.

```bash
aws cloudformation list-stack-sets
```

## To get stack set details

The following example gets stack set details.

```bash
aws cloudformation describe-stack-set --stack-set-name my-stackset
```

## To create a stack set

The following example creates a stack set.

```bash
aws cloudformation create-stack-set
    --stack-set-name my-stackset
    --template-body file://template.json
```

## To create with execution role

The following example creates with execution role.

```bash
aws cloudformation create-stack-set
    --stack-set-name my-stackset
    --template-body file://template.json
    --execution-role-name CloudFormationExecutionRole
```

## To delete a stack set

The following example deletes a stack set.

```bash
aws cloudformation delete-stack-set --stack-set-name my-stackset
```

## To deploy to accounts/regions

The following example deploys to accounts/regions.

```bash
aws cloudformation create-stack-instances
    --stack-set-name my-stackset
    --accounts '["123456789012","987654321098"]'
    --regions '["us-east-1","us-west-2"]'
```

## To get stack instance details

The following example gets stack instance details.

```bash
aws cloudformation describe-stack-instance
    --stack-set-name my-stackset
    --stack-instance-account 123456789012
    --stack-instance-region us-east-1
```

## To list all stack instances

The following example lists all stack instances.

```bash
aws cloudformation list-stack-instances --stack-set-name my-stackset
```

## To delete stack instances

The following example deletes stack instances.

```bash
aws cloudformation delete-stack-instances
    --stack-set-name my-stackset
    --accounts '["123456789012"]'
    --regions '["us-east-1"]'
```

## To update stack instances

The following example updates stack instances.

```bash
aws cloudformation update-stack-instances
    --stack-set-name my-stackset
    --accounts '["123456789012"]'
    --regions '["us-east-1"]'
    --template-body file://template-v2.json
```
