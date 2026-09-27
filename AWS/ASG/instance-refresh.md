# Instance Refresh

> Instance Refresh. Part of the [ASG](../ASG.md) cheatsheet.

## To start an instance refresh (rolling update)

The following example starts an instance refresh (rolling update).

```bash
aws autoscaling start-instance-refresh --auto-scaling-group-name my-asg
```

## To check instance refresh status

The following example checks instance refresh status.

```bash
aws autoscaling describe-instance-refreshes --auto-scaling-group-name my-asg
```
