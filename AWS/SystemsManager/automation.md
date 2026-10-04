# Automation

> Automation. Part of the [SystemsManager](../) cheatsheet.

## To list automation documents

The following example lists automation documents.

```bash
aws ssm list-automations
```

## To start automation

The following example starts automation.

```bash
aws ssm start-automation-execution
    --document-name "AWS-StartEC2Instance"
    --parameters InstanceId=i-xxx
```

## To list automation executions

The following example lists automation executions.

```bash
aws ssm describe-automation-executions
```

## To get step details

The following example gets step details.

```bash
aws ssm describe-automation-step-executions --automation-execution-id xxx
```
