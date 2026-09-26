# CloudFormation (Infrastructure as Code)

> Commands for stacks, change sets, StackSets, exports, and drift detection.

## Stack Operations

### To list active stacks

The following example lists active stacks.

```bash
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE
```

### To get stack details

The following example gets stack details.

```bash
aws cloudformation describe-stacks --stack-name my-stack
```

### To create a stack

The following example creates a stack.

```bash
aws cloudformation create-stack --stack-name my-stack --template-body file://template.json
```

### To create from S3 template

The following example creates from S3 template.

```bash
aws cloudformation create-stack
    --stack-name my-stack
    --template-url https://s3.amazonaws.com/my-bucket/template.json
```

### To create with parameters

The following example creates with parameters.

```bash
aws cloudformation create-stack
    --stack-name my-stack
    --template-body file://template.json
    --parameters ParameterKey=Env,ParameterValue=prod
```

### To update a stack

The following example updates a stack.

```bash
aws cloudformation update-stack --stack-name my-stack --template-body file://template-v2.json
```

### To delete a stack

The following example deletes a stack.

```bash
aws cloudformation delete-stack --stack-name my-stack
```

### To cancel an in-progress update

The following example cancels an in-progress update.

```bash
aws cloudformation cancel-update-stack --stack-name my-stack
```

### To view stack events

The following example views stack events.

```bash
aws cloudformation describe-stack-events --stack-name my-stack
```

### To get current template

The following example gets current template.

```bash
aws cloudformation get-template --stack-name my-stack
```

## Stack Resources

### To list all resources in a stack

The following example lists all resources in a stack.

```bash
aws cloudformation list-stack-resources --stack-name my-stack
```

### To get details for a specific resource

The following example gets details for a specific resource.

```bash
aws cloudformation describe-stack-resource --stack-name my-stack --logical-resource-id MyEC2Instance
```

### To find resource by physical ID

The following example finds resource by physical ID.

```bash
aws cloudformation describe-stack-resources --stack-name my-stack --physical-resource-id i-xxxxxxxx
```

### To continue rollback after failure

The following example continues rollback after failure.

```bash
aws cloudformation continue-update-rollback --stack-name my-stack
```

## Change Sets

### To create a change set

The following example creates a change set.

```bash
aws cloudformation create-change-set
    --stack-name my-stack
    --change-set-name my-changes
    --template-body file://template-v2.json
```

### To create change set for new stack

The following example creates change set for new stack.

```bash
aws cloudformation create-change-set
    --stack-name my-stack
    --change-set-name my-changes
    --template-body file://template.json
    --change-set-type CREATE
```

### To view change set details

The following example views change set details.

```bash
aws cloudformation describe-change-set --change-set-name my-changes --stack-name my-stack
```

### To list change sets

The following example lists change sets.

```bash
aws cloudformation list-change-sets --stack-name my-stack
```

### To execute a change set

The following example executes a change set.

```bash
aws cloudformation execute-change-set --change-set-name my-changes --stack-name my-stack
```

### To delete a change set

The following example deletes a change set.

```bash
aws cloudformation delete-change-set --change-set-name my-changes --stack-name my-stack
```

## Stack Sets

### To list all stack sets

The following example lists all stack sets.

```bash
aws cloudformation list-stack-sets
```

### To get stack set details

The following example gets stack set details.

```bash
aws cloudformation describe-stack-set --stack-set-name my-stackset
```

### To create a stack set

The following example creates a stack set.

```bash
aws cloudformation create-stack-set
    --stack-set-name my-stackset
    --template-body file://template.json
```

### To create with execution role

The following example creates with execution role.

```bash
aws cloudformation create-stack-set
    --stack-set-name my-stackset
    --template-body file://template.json
    --execution-role-name CloudFormationExecutionRole
```

### To delete a stack set

The following example deletes a stack set.

```bash
aws cloudformation delete-stack-set --stack-set-name my-stackset
```

### To deploy to accounts/regions

The following example deploys to accounts/regions.

```bash
aws cloudformation create-stack-instances
    --stack-set-name my-stackset
    --accounts '["123456789012","987654321098"]'
    --regions '["us-east-1","us-west-2"]'
```

### To get stack instance details

The following example gets stack instance details.

```bash
aws cloudformation describe-stack-instance
    --stack-set-name my-stackset
    --stack-instance-account 123456789012
    --stack-instance-region us-east-1
```

### To list all stack instances

The following example lists all stack instances.

```bash
aws cloudformation list-stack-instances --stack-set-name my-stackset
```

### To delete stack instances

The following example deletes stack instances.

```bash
aws cloudformation delete-stack-instances
    --stack-set-name my-stackset
    --accounts '["123456789012"]'
    --regions '["us-east-1"]'
```

### To update stack instances

The following example updates stack instances.

```bash
aws cloudformation update-stack-instances
    --stack-set-name my-stackset
    --accounts '["123456789012"]'
    --regions '["us-east-1"]'
    --template-body file://template-v2.json
```

## Exports & Outputs

### To list all exports

The following example lists all exports.

```bash
aws cloudformation list-exports
```

### To find stacks using an export

The following example finds stacks using an export.

```bash
aws cloudformation list-imports --export-name my-export
```

## Validate & Estimate

### To validate a template

The following example validates a template.

```bash
aws cloudformation validate-template --template-body file://template.json
```

### To estimate template cost

The following example estimates template cost.

```bash
aws cloudformation estimate-template-cost --template-body file://template.json
```

### To get template summary

The following example gets template summary.

```bash
aws cloudformation get-template-summary --template-body file://template.json
```

## Drift Detection

### To start drift detection

The following example starts drift detection.

```bash
aws cloudformation detect-stack-drift --stack-name my-stack
```

### To check drift status

The following example checks drift status.

```bash
aws cloudformation describe-stack-drift-detection-status --stack-drift-detection-id xxx
```

### To list drifted resources

The following example lists drifted resources.

```bash
aws cloudformation describe-stack-resource-drifts --stack-name my-stack
```
