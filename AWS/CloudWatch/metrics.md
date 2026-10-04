# Metrics

> Metrics. Part of the [CloudWatch](../) cheatsheet.

## To list EC2 metrics

The following example lists EC2 metrics.

```bash
aws cloudwatch list-metrics --namespace AWS/EC2
```

## To list specific metrics

The following example lists specific metrics.

```bash
aws cloudwatch list-metrics --namespace AWS/EC2 --metric-name CPUUtilization
```

## To get metric statistics

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

## To publish custom metric

The following example publishes custom metric.

```bash
aws cloudwatch put-metric-data --namespace MyApp --metric-name RequestCount --value 100
```

## To publish with dimensions

The following example publishes with dimensions.

```bash
aws cloudwatch put-metric-data
    --namespace MyApp
    --metric-name RequestCount
    --value 100
    --dimensions Environment=prod,Service=api
```
