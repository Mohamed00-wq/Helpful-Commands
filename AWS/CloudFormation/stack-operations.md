# Stack Operations

> Stack Operations. Part of the [CloudFormation](../CloudFormation.md) cheatsheet.

## To list active stacks

The following example lists active stacks.

```bash
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE
```

## To get stack details

The following example gets stack details.

```bash
aws cloudformation describe-stacks --stack-name my-stack
```

## To create a stack

The following example creates a stack.

```bash
aws cloudformation create-stack --stack-name my-stack --template-body file://template.json
```

## To create from S3 template

The following example creates from S3 template.

```bash
aws cloudformation create-stack
    --stack-name my-stack
    --template-url https://s3.amazonaws.com/my-bucket/template.json
```

## To create with parameters

The following example creates with parameters.

```bash
aws cloudformation create-stack
    --stack-name my-stack
    --template-body file://template.json
    --parameters ParameterKey=Env,ParameterValue=prod
```

## To update a stack

The following example updates a stack.

```bash
aws cloudformation update-stack --stack-name my-stack --template-body file://template-v2.json
```

## To delete a stack

The following example deletes a stack.

```bash
aws cloudformation delete-stack --stack-name my-stack
```

## To cancel an in-progress update

The following example cancels an in-progress update.

```bash
aws cloudformation cancel-update-stack --stack-name my-stack
```

## To view stack events

The following example views stack events.

```bash
aws cloudformation describe-stack-events --stack-name my-stack
```

## To get current template

The following example gets current template.

```bash
aws cloudformation get-template --stack-name my-stack
```
