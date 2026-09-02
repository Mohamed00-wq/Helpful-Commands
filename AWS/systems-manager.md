# ⚙️ Systems Manager (SSM)

> Advanced SSM CLI commands for sessions, patching, automation, and parameter store — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Instances (SSM Agent)

| Command | Description |
|---|---|
| `aws ssm describe-instance-information` | List managed instances |
| `aws ssm describe-instance-information --filters "Key=PlatformTypes,Values=Windows"` | Filter by platform |
| `aws ssm send-command --instance-ids i-xxx --document-name "AWS-RunShellScript" --parameters commands=["uptime"]` | Run shell command |
| `aws ssm send-command --instance-ids i-xxx --document-name "AWS-RunPowerShellScript" --parameters commands=["Get-Process"]` | Run PowerShell |
| `aws ssm list-command-invocations --command-id xxx` | Get command results |
| `aws ssm get-command-invocation --command-id xxx --instance-id i-xxx` | Get specific instance result |

```bash
# Run command on instance
aws ssm send-command \
  --instance-ids i-xxx \
  --document-name "AWS-RunShellScript" \
  --parameters commands=["uptime","df -h"]

# Check results
aws ssm list-command-invocations --command-id xxx
```

---

## Session Manager

| Command | Description |
|---|---|
| `aws ssm start-session --target i-xxx` | Start a session |
| `aws ssm terminate-session --session-id xxx` | Terminate a session |
| `aws ssm describe-sessions --state Active` | List active sessions |

```bash
# Start interactive session
aws ssm start-session --target i-xxx

# List active sessions
aws ssm describe-sessions --state Active
```

---

## Patch Management

| Command | Description |
|---|---|
| `aws ssm describe-patch-groups` | List patch groups |
| `aws ssm describe-patch-baselines` | List patch baselines |
| `aws ssm create-patch-baseline --name my-baseline --operating-system AMAZON_LINUX_2` | Create a patch baseline |
| `aws ssm register-default-patch-baseline --baseline-id pb-xxx` | Set default baseline |
| `aws ssm scan-patches --instance-ids i-xxx` | Scan for patches |
| `aws ssm describe-patches --filters "Key=CLASSIFICATION,Values=Security"` | List patches |
| `aws ssm install-patches --instance-ids i-xxx --baseline-id pb-xxx --operation RebootIfNeeded` | Install patches |
| `aws ssm describe-patch-installations` | List patch installations |

```bash
# Create patch baseline
aws ssm create-patch-baseline \
  --name my-baseline \
  --operating-system AMAZON_LINUX_2

# Scan for patches
aws ssm scan-patches --instance-ids i-xxx

# Install patches
aws ssm install-patches \
  --instance-ids i-xxx \
  --baseline-id pb-xxx \
  --operation RebootIfNeeded
```

---

## Automation

| Command | Description |
|---|---|
| `aws ssm list-automations` | List automation documents |
| `aws ssm start-automation-execution --document-name "AWS-StartEC2Instance" --parameters InstanceId=i-xxx` | Start automation |
| `aws ssm describe-automation-executions` | List automation executions |
| `aws ssm describe-automation-step-executions --automation-execution-id xxx` | Get step details |

```bash
# Start automation
aws ssm start-automation-execution \
  --document-name "AWS-StartEC2Instance" \
  --parameters InstanceId=i-xxx
```

---

## Maintenance Windows

| Command | Description |
|---|---|
| `aws ssm describe-maintenance-windows` | List maintenance windows |
| `aws ssm create-maintenance-window --name my-window --schedule "cron(0 2 ? * SUN *)" --duration 4 --cutoff 1` | Create a window |
| `aws ssm register-target-with-maintenance-window --window-id mw-xxx --resource-type INSTANCE --targets "Key=tag:env,Values=prod"` | Register targets |
| `aws ssm register-task-with-maintenance-window --window-id mw-xxx --task-type RUN_COMMAND --task-arn "AWS-RunShellScript" --targets "Key=WindowTargetIds,Values=xxx"` | Register task |
| `aws ssm describe-maintenance-window-executions --window-id mw-xxx` | List window executions |

```bash
# Create maintenance window
aws ssm create-maintenance-window \
  --name my-window \
  --schedule "cron(0 2 ? * SUN *)" \
  --duration 4 \
  --cutoff 1
```

---

## State Manager

| Command | Description |
|---|---|
| `aws ssm list-associations` | List associations |
| `aws ssm create-association --name "AWS-RunShellScript" --targets "Key=tag:env,Values=prod" --parameters commands=["yum update -y"]` | Create association |
| `aws ssm describe-association --association-id xxx` | Get association details |
| `aws ssm delete-association --association-id xxx` | Delete association |

```bash
# Auto-update all prod instances
aws ssm create-association \
  --name "AWS-RunShellScript" \
  --targets "Key=tag:env,Values=prod" \
  --parameters commands=["yum update -y"]
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
