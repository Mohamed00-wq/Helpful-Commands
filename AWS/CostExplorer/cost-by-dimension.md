# Cost by Dimension

> Cost by Dimension. Part of the [CostExplorer](../CostExplorer.md) cheatsheet.

Grouping splits the cost into one bucket per value of the dimension, and each
bucket appears under `Groups` in every time bucket of the response.

## To get the cost of every service

The following example groups the month by service.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=SERVICE
```

## To get the cost of every Region

The following example groups the month by Region.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=REGION
```

## To get the cost by usage type

The following example groups the month by usage type, which is the level to drop
to when a service line item is too coarse to explain.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=USAGE_TYPE
```

## To get the cost of every value of one tag

The following example groups by tag, and a tag bucket only appears for resources
that carry the tag, so the buckets never add up to the account total.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=TAG,Key=Environment
```

## To get the cost of a service and a Region together

The following example adds both groupings, and each bucket then carries the
service in `Keys[0]` and the Region in `Keys[1]`.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=SERVICE Type=DIMENSION,Key=REGION
```
