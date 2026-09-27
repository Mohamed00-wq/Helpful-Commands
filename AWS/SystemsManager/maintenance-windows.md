# Maintenance Windows

> Maintenance Windows. Part of the [SystemsManager](../SystemsManager.md) cheatsheet.

## To list maintenance windows

The following example lists maintenance windows.

```bash
aws ssm describe-maintenance-windows
```

## To create a window

The following example creates a window.

```bash
aws ssm create-maintenance-window
    --name my-window
    --schedule "cron(0 2 ? * SUN *)"
    --duration 4
    --cutoff 1
```

## To register targets

The following example registers targets.

```bash
aws ssm register-target-with-maintenance-window
    --window-id mw-xxx
    --resource-type INSTANCE
    --targets "Key=tag:env,Values=prod"
```

## To register task

The following example registers task.

```bash
aws ssm register-task-with-maintenance-window
    --window-id mw-xxx
    --task-type RUN_COMMAND
    --task-arn "AWS-RunShellScript"
    --targets "Key=WindowTargetIds,Values=xxx"
```

## To list window executions

The following example lists window executions.

```bash
aws ssm describe-maintenance-window-executions --window-id mw-xxx
```
