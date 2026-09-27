# Dashboards

> Dashboards. Part of the [CloudWatch](../CloudWatch.md) cheatsheet.

## To list all dashboards

The following example lists all dashboards.

```bash
aws cloudwatch list-dashboards
```

## To get dashboard JSON

The following example gets dashboard JSON.

```bash
aws cloudwatch get-dashboard --dashboard-name my-dashboard
```

## To create or update a dashboard

The following example creates a dashboard, or replaces it in full if it already exists.

```bash
aws cloudwatch put-dashboard --dashboard-name my-dashboard --dashboard-body file://dashboard.json
```

## To delete a dashboard

The following example deletes a dashboard.

```bash
aws cloudwatch delete-dashboards --dashboard-names my-dashboard
```
