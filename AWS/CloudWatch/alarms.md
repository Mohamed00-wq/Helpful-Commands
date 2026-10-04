# Alarms

> Alarms. Part of the [CloudWatch](../) cheatsheet.

## To list all alarms

## To get alarm details

The following example gets alarm details.

```bash
aws cloudwatch describe-alarms --alarm-names my-alarm
```

## To create a CloudWatch alarm

The following example creates a CloudWatch alarm.

```bash
aws cloudwatch put-metric-alarm
    --alarm-name high-cpu
    --metric-name CPUUtilization
    --namespace AWS/EC2
    --statistic Average
    --period 300
    --threshold 80
    --comparison-operator GreaterThanThreshold
    --evaluation-periods 2
    --dimensions "Name=InstanceId,Value=i-xxxxxxxx"
```

## To delete an alarm

The following example deletes an alarm.

```bash
aws cloudwatch delete-alarms --alarm-names my-alarm
```

## To set alarm state manually

The following example sets alarm state manually.

```bash
aws cloudwatch set-alarm-state --alarm-name my-alarm --state-value ALARM --state-reason "Testing"
```

## To enable alarm actions

The following example enables alarm actions.

```bash
aws cloudwatch enable-alarm-actions --alarm-names my-alarm
```

## To disable alarm actions

The following example disables alarm actions.

```bash
aws cloudwatch disable-alarm-actions --alarm-names my-alarm
```
