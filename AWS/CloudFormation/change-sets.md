# Change Sets

> Change Sets. Part of the [CloudFormation](../CloudFormation.md) cheatsheet.

## To create a change set

The following example creates a change set.

```bash
aws cloudformation create-change-set
    --stack-name my-stack
    --change-set-name my-changes
    --template-body file://template-v2.json
```

## To create change set for new stack

The following example creates change set for new stack.

```bash
aws cloudformation create-change-set
    --stack-name my-stack
    --change-set-name my-changes
    --template-body file://template.json
    --change-set-type CREATE
```

## To view change set details

The following example views change set details.

```bash
aws cloudformation describe-change-set --change-set-name my-changes --stack-name my-stack
```

## To list change sets

The following example lists change sets.

```bash
aws cloudformation list-change-sets --stack-name my-stack
```

## To execute a change set

The following example executes a change set.

```bash
aws cloudformation execute-change-set --change-set-name my-changes --stack-name my-stack
```

## To delete a change set

The following example deletes a change set.

```bash
aws cloudformation delete-change-set --change-set-name my-changes --stack-name my-stack
```
