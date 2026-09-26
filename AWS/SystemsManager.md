# Systems Manager (SSM)

> Commands for sessions, patching, automation runbooks, and maintenance windows.

## Instances (SSM Agent)

### To list managed instances

### To filter by platform

The following example filters by platform.

```bash
aws ssm describe-instance-information --filters "Key=PlatformTypes,Values=Windows"
```

### To run shell command

The following example runs shell command.

```bash
aws ssm send-command
    --instance-ids i-xxx
    --document-name "AWS-RunShellScript"
    --parameters commands=["uptime"]
```

### To run PowerShell

The following example runs PowerShell.

```bash
aws ssm send-command
    --instance-ids i-xxx
    --document-name "AWS-RunPowerShellScript"
    --parameters commands=["Get-Process"]
```

### To get command results

The following example gets command results.

```bash
aws ssm list-command-invocations --command-id xxx
```

### To get specific instance result

The following example gets specific instance result.

```bash
aws ssm get-command-invocation --command-id xxx --instance-id i-xxx
```

```bash
# Run command on instance
aws ssm send-command \
    --instance-ids i-xxx \
    --document-name "AWS-RunShellScript" \
    --parameters commands=["uptime","df -h"]

# Check results
aws ssm list-command-invocations --command-id xxx
```

## Session Manager

### To start a session

The following example starts a session.

```bash
aws ssm start-session --target i-xxx
```

### To terminate a session

The following example terminates a session.

```bash
aws ssm terminate-session --session-id xxx
```

### To list active sessions

The following example lists active sessions.

```bash
aws ssm describe-sessions --state Active
```

## Patch Management

### To list patch groups

The following example lists patch groups.

```bash
aws ssm describe-patch-groups
```

### To list patch baselines

The following example lists patch baselines.

```bash
aws ssm describe-patch-baselines
```

### To create a patch baseline

The following example creates a patch baseline.

```bash
aws ssm create-patch-baseline --name my-baseline --operating-system AMAZON_LINUX_2
```

### To set default baseline

The following example sets default baseline.

```bash
aws ssm register-default-patch-baseline --baseline-id pb-xxx
```

### To scan for patches

The following example scans for patches.

```bash
aws ssm scan-patches --instance-ids i-xxx
```

### To list patches

The following example lists patches.

```bash
aws ssm describe-patches --filters "Key=CLASSIFICATION,Values=Security"
```

### To install patches

The following example installs patches.

```bash
aws ssm install-patches --instance-ids i-xxx --baseline-id pb-xxx --operation RebootIfNeeded
```

### To list patch installations

The following example lists patch installations.

```bash
aws ssm describe-patch-installations
```

## Automation

### To list automation documents

The following example lists automation documents.

```bash
aws ssm list-automations
```

### To start automation

The following example starts automation.

```bash
aws ssm start-automation-execution
    --document-name "AWS-StartEC2Instance"
    --parameters InstanceId=i-xxx
```

### To list automation executions

The following example lists automation executions.

```bash
aws ssm describe-automation-executions
```

### To get step details

The following example gets step details.

```bash
aws ssm describe-automation-step-executions --automation-execution-id xxx
```

## Maintenance Windows

### To list maintenance windows

The following example lists maintenance windows.

```bash
aws ssm describe-maintenance-windows
```

### To create a window

The following example creates a window.

```bash
aws ssm create-maintenance-window
    --name my-window
    --schedule "cron(0 2 ? * SUN *)"
    --duration 4
    --cutoff 1
```

### To register targets

The following example registers targets.

```bash
aws ssm register-target-with-maintenance-window
    --window-id mw-xxx
    --resource-type INSTANCE
    --targets "Key=tag:env,Values=prod"
```

### To register task

The following example registers task.

```bash
aws ssm register-task-with-maintenance-window
    --window-id mw-xxx
    --task-type RUN_COMMAND
    --task-arn "AWS-RunShellScript"
    --targets "Key=WindowTargetIds,Values=xxx"
```

### To list window executions

The following example lists window executions.

```bash
aws ssm describe-maintenance-window-executions --window-id mw-xxx
```

## State Manager

### To list associations

The following example lists associations.

```bash
aws ssm list-associations
```

### To create association

The following example creates association.

```bash
aws ssm create-association
    --name "AWS-RunShellScript"
    --targets "Key=tag:env,Values=prod"
    --parameters commands=["yum update
    -y"]
```

### To get association details

The following example gets association details.

```bash
aws ssm describe-association --association-id xxx
```

### To delete association

The following example deletes association.

```bash
aws ssm delete-association --association-id xxx
```
