# CloudWatch (Monitoring and Observability)

> Commands for metrics, alarms, logs, dashboards, and metric filters.

## Alarms

### To list all alarms

### To get alarm details

The following example gets alarm details.

```bash
aws cloudwatch describe-alarms --alarm-names my-alarm
```

### To create a CloudWatch alarm

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

### To delete an alarm

The following example deletes an alarm.

```bash
aws cloudwatch delete-alarms --alarm-names my-alarm
```

### To set alarm state manually

The following example sets alarm state manually.

```bash
aws cloudwatch set-alarm-state --alarm-name my-alarm --state-value ALARM --state-reason "Testing"
```

### To enable alarm actions

The following example enables alarm actions.

```bash
aws cloudwatch enable-alarm-actions --alarm-names my-alarm
```

### To disable alarm actions

The following example disables alarm actions.

```bash
aws cloudwatch disable-alarm-actions --alarm-names my-alarm
```

## Metrics

### To list EC2 metrics

The following example lists EC2 metrics.

```bash
aws cloudwatch list-metrics --namespace AWS/EC2
```

### To list specific metrics

The following example lists specific metrics.

```bash
aws cloudwatch list-metrics --namespace AWS/EC2 --metric-name CPUUtilization
```

### To get metric statistics

The following example gets metric statistics.

```bash
aws cloudwatch get-metric-statistics
    --namespace AWS/EC2
    --metric-name CPUUtilization
    --dimensions Name=InstanceId,Value=i-xxxxxxxx
    --start-time 2026-01-15T00:00:00Z
    --end-time 2026-01-15T23:59:59Z
    --period 300
    --statistics Average
```

### To publish custom metric

The following example publishes custom metric.

```bash
aws cloudwatch put-metric-data --namespace MyApp --metric-name RequestCount --value 100
```

### To publish with dimensions

The following example publishes with dimensions.

```bash
aws cloudwatch put-metric-data
    --namespace MyApp
    --metric-name RequestCount
    --value 100
    --dimensions Environment=prod,Service=api
```

## Log Groups

### To list all log groups

### To filter log groups

The following example filters log groups.

```bash
aws logs describe-log-groups --log-group-name-prefix /aws/lambda
```

### To create a log group

The following example creates a log group.

```bash
aws logs create-log-group --log-group-name /my/app
```

### To delete a log group

The following example deletes a log group.

```bash
aws logs delete-log-group --log-group-name /my/app
```

### To set retention policy

The following example sets retention policy.

```bash
aws logs put-retention-policy --log-group-name /my/app --retention-in-days 30
```

### To remove retention (infinite)

The following example removes retention (infinite).

```bash
aws logs delete-retention-policy --log-group-name /my/app
```

## Log Streams & Events

### To list log streams

The following example lists log streams.

```bash
aws logs describe-log-streams --log-group-name /my/app
```

### To get log events

The following example gets log events.

```bash
aws logs describe-log-events --log-group-name /my/app --log-stream-name my-stream
```

### To get events with filter

The following example gets events with filter.

```bash
aws logs describe-log-events
    --log-group-name /my/app
    --log-stream-name my-stream
    --start-time 1705276800000
    --filter-pattern "ERROR"
```

### To search across all streams

The following example searches across all streams.

```bash
aws logs filter-log-events
    --log-group-name /my/app
    --filter-pattern "ERROR"
    --start-time 1705276800000
```

### To put log events

The following example puts log events.

```bash
aws logs put-log-events
    --log-group-name /my/app
    --log-stream-name my-stream
    --log-events file://events.json
```

## Metric Filters

### To create a metric filter

The following example creates a metric filter.

```bash
aws logs put-metric-filter
    --log-group-name /my/app
    --filter-name ErrorCount
    --filter-pattern "ERROR"
    --metric-transformations metricName=ErrorCount,metricNamespace=MyApp,metricValue=1
```

### To list metric filters

The following example lists metric filters.

```bash
aws logs describe-metric-filters --log-group-name /my/app
```

### To delete a metric filter

The following example deletes a metric filter.

```bash
aws logs delete-metric-filter --log-group-name /my/app --filter-name ErrorCount
```

## Subscription Filters

### To subscribe to log events

The following example subscribes to log events.

```bash
aws logs put-subscription-filter
    --log-group-name /my/app
    --filter-name my-filter
    --destination-arn arn:aws:lambda:us-east-1:123456789012:function:my-function
    --filter-pattern ""
```

### To list subscription filters

The following example lists subscription filters.

```bash
aws logs describe-subscription-filters --log-group-name /my/app
```

### To delete subscription filter

The following example deletes subscription filter.

```bash
aws logs delete-subscription-filter --log-group-name /my/app --filter-name my-filter
```

## Dashboards

### To list all dashboards

The following example lists all dashboards.

```bash
aws cloudwatch list-dashboards
```

### To get dashboard JSON

The following example gets dashboard JSON.

```bash
aws cloudwatch get-dashboard --dashboard-name my-dashboard
```

### To create or update a dashboard

The following example creates a dashboard, or replaces it in full if it already exists.

```bash
aws cloudwatch put-dashboard --dashboard-name my-dashboard --dashboard-body file://dashboard.json
```

### To delete a dashboard

The following example deletes a dashboard.

```bash
aws cloudwatch delete-dashboards --dashboard-names my-dashboard
```
