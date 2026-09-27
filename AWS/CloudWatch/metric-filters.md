# Metric Filters

> Metric Filters. Part of the [CloudWatch](../CloudWatch.md) cheatsheet.

## To create a metric filter

The following example creates a metric filter.

```bash
aws logs put-metric-filter
    --log-group-name /my/app
    --filter-name ErrorCount
    --filter-pattern "ERROR"
    --metric-transformations metricName=ErrorCount,metricNamespace=MyApp,metricValue=1
```

## To list metric filters

The following example lists metric filters.

```bash
aws logs describe-metric-filters --log-group-name /my/app
```

## To delete a metric filter

The following example deletes a metric filter.

```bash
aws logs delete-metric-filter --log-group-name /my/app --filter-name ErrorCount
```
