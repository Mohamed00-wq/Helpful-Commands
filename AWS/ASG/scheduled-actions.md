# Scheduled Actions

> Scheduled Actions. Part of the [ASG](../) cheatsheet.

## To schedule a scaling action

The following example schedules a scaling action.

```bash
aws autoscaling put-scheduled-update-group-action
    --auto-scaling-group-name my-asg
    --scheduled-action-name scale-up-morning
    --recurrence "0 8 * * *"
    --min-size 4
    --max-size 8
    --desired-capacity 5
```

## To list scheduled actions

The following example lists scheduled actions.

```bash
aws autoscaling describe-scheduled-actions --auto-scaling-group-name my-asg
```

## To delete a scheduled action

The following example deletes a scheduled action.

```bash
aws autoscaling delete-scheduled-action
    --auto-scaling-group-name my-asg
    --scheduled-action-name scale-up-morning
```
