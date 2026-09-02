# 📊 CloudWatch (Monitoring & Observability)

> Advanced CloudWatch CLI commands for alarms, metrics, logs, and dashboards — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Alarms

| Command | Description |
|---|---|
| `aws cloudwatch describe-alarms` | List all alarms |
| `aws cloudwatch describe-alarms --alarm-names my-alarm` | Get alarm details |
| `aws cloudwatch put-metric-alarm --alarm-name high-cpu --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 80 --comparison-operator GreaterThanThreshold --evaluation-periods 2 --dimensions "Name=InstanceId,Value=i-xxxxxxxx"` | Create a CloudWatch alarm |
| `aws cloudwatch delete-alarms --alarm-names my-alarm` | Delete an alarm |
| `aws cloudwatch set-alarm-state --alarm-name my-alarm --state-value ALARM --state-reason "Testing"` | Set alarm state manually |
| `aws cloudwatch enable-alarm-actions --alarm-names my-alarm` | Enable alarm actions |
| `aws cloudwatch disable-alarm-actions --alarm-names my-alarm` | Disable alarm actions |

```bash
# Create CPU alarm
aws cloudwatch put-metric-alarm \
  --alarm-name high-cpu \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --dimensions "Name=InstanceId,Value=i-xxxxxxxx"

# Manual state change
aws cloudwatch set-alarm-state \
  --alarm-name my-alarm \
  --state-value ALARM \
  --state-reason "Testing"
```

---

## Metrics

| Command | Description |
|---|---|
| `aws cloudwatch list-metrics --namespace AWS/EC2` | List EC2 metrics |
| `aws cloudwatch list-metrics --namespace AWS/EC2 --metric-name CPUUtilization` | List specific metrics |
| `aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-xxxxxxxx --start-time 2026-01-15T00:00:00Z --end-time 2026-01-15T23:59:59Z --period 300 --statistics Average` | Get metric statistics |
| `aws cloudwatch put-metric-data --namespace MyApp --metric-name RequestCount --value 100` | Publish custom metric |
| `aws cloudwatch put-metric-data --namespace MyApp --metric-name RequestCount --value 100 --dimensions Environment=prod,Service=api` | Publish with dimensions |

```bash
# Get CPU stats
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-xxxxxxxx \
  --start-time 2026-01-15T00:00:00Z \
  --end-time 2026-01-15T23:59:59Z \
  --period 300 \
  --statistics Average

# Publish custom metric
aws cloudwatch put-metric-data \
  --namespace MyApp \
  --metric-name RequestCount \
  --value 100 \
  --dimensions Environment=prod,Service=api
```

---

## Log Groups

| Command | Description |
|---|---|
| `aws logs describe-log-groups` | List all log groups |
| `aws logs describe-log-groups --log-group-name-prefix /aws/lambda` | Filter log groups |
| `aws logs create-log-group --log-group-name /my/app` | Create a log group |
| `aws logs delete-log-group --log-group-name /my/app` | Delete a log group |
| `aws logs put-retention-policy --log-group-name /my/app --retention-in-days 30` | Set retention policy |
| `aws logs delete-retention-policy --log-group-name /my/app` | Remove retention (infinite) |

```bash
# Create log group
aws logs create-log-group --log-group-name /my/app

# Set retention
aws logs put-retention-policy \
  --log-group-name /my/app \
  --retention-in-days 30
```

---

## Log Streams & Events

| Command | Description |
|---|---|
| `aws logs describe-log-streams --log-group-name /my/app` | List log streams |
| `aws logs describe-log-events --log-group-name /my/app --log-stream-name my-stream` | Get log events |
| `aws logs describe-log-events --log-group-name /my/app --log-stream-name my-stream --start-time 1705276800000 --filter-pattern "ERROR"` | Get events with filter |
| `aws logs filter-log-events --log-group-name /my/app --filter-pattern "ERROR" --start-time 1705276800000` | Search across all streams |
| `aws logs put-log-events --log-group-name /my/app --log-stream-name my-stream --log-events file://events.json` | Put log events |

```bash
# Search for errors across all streams
aws logs filter-log-events \
  --log-group-name /my/app \
  --filter-pattern "ERROR" \
  --start-time 1705276800000
```

---

## Metric Filters

| Command | Description |
|---|---|
| `aws logs put-metric-filter --log-group-name /my/app --filter-name ErrorCount --filter-pattern "ERROR" --metric-transformations metricName=ErrorCount,metricNamespace=MyApp,metricValue=1` | Create a metric filter |
| `aws logs describe-metric-filters --log-group-name /my/app` | List metric filters |
| `aws logs delete-metric-filter --log-group-name /my/app --filter-name ErrorCount` | Delete a metric filter |

```bash
aws logs put-metric-filter \
  --log-group-name /my/app \
  --filter-name ErrorCount \
  --filter-pattern "ERROR" \
  --metric-transformations metricName=ErrorCount,metricNamespace=MyApp,metricValue=1
```

---

## Subscription Filters

| Command | Description |
|---|---|
| `aws logs put-subscription-filter --log-group-name /my/app --filter-name my-filter --destination-arn arn:aws:lambda:us-east-1:123456789012:function:my-function --filter-pattern ""` | Subscribe to log events |
| `aws logs describe-subscription-filters --log-group-name /my/app` | List subscription filters |
| `aws logs delete-subscription-filter --log-group-name /my/app --filter-name my-filter` | Delete subscription filter |

```bash
aws logs put-subscription-filter \
  --log-group-name /my/app \
  --filter-name my-filter \
  --destination-arn arn:aws:lambda:us-east-1:123456789012:function:my-function \
  --filter-pattern ""
```

---

## Dashboards

| Command | Description |
|---|---|
| `aws cloudwatch list-dashboards` | List all dashboards |
| `aws cloudwatch get-dashboard --dashboard-name my-dashboard` | Get dashboard JSON |
| `aws cloudwatch put-dashboard --dashboard-name my-dashboard --dashboard-body file://dashboard.json` | Create/update dashboard |
| `aws cloudwatch delete-dashboards --dashboard-names my-dashboard` | Delete a dashboard |

```bash
aws cloudwatch put-dashboard \
  --dashboard-name my-dashboard \
  --dashboard-body file://dashboard.json
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
