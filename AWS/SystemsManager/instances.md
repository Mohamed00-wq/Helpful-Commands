# Instances (SSM Agent)

> Instances (SSM Agent). Part of the [SystemsManager](../SystemsManager.md) cheatsheet.

## To list managed instances

## To filter by platform

The following example filters by platform.

```bash
aws ssm describe-instance-information --filters "Key=PlatformTypes,Values=Windows"
```

## To run shell command

The following example runs shell command.

```bash
aws ssm send-command
    --instance-ids i-xxx
    --document-name "AWS-RunShellScript"
    --parameters commands=["uptime"]
```

## To run PowerShell

The following example runs PowerShell.

```bash
aws ssm send-command
    --instance-ids i-xxx
    --document-name "AWS-RunPowerShellScript"
    --parameters commands=["Get-Process"]
```

## To get command results

The following example gets command results.

```bash
aws ssm list-command-invocations --command-id xxx
```

## To get specific instance result

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
