# Forecasts

> Forecasts. Part of the [CostExplorer](../) cheatsheet.

## To forecast a month's cost

The following example forecasts one month from the usage recorded in it.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY
```

## To forecast with a confidence band

The following example asks for an 80 percent prediction interval, which is what
you want before you commit a budget number rather than the mean alone.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY \
    --prediction-interval-level 80
```

## To read the forecast mean

The following example prints the mean forecast value as text, and the buckets
sit under `ForecastResultsByTime` rather than under a `ForecastResults` key.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY \
    --query 'ForecastResultsByTime[0].MeanValue' --output text
```

## To forecast the cost of one service

The following example narrows the forecast to a single service, and the metric
name here is upper case with underscores, unlike the `--metrics` values in
[Granularity and Metrics](granularity-and-metrics.md).

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY \
    --filter 'Dimensions={Key=SERVICE,Values=AmazonEC2}'
```
