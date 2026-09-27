# Granularity and Metrics

> Granularity and Metrics. Part of the [CostExplorer](../CostExplorer.md) cheatsheet.

`--granularity` takes `DAILY`, `MONTHLY`, or `HOURLY`, and the value decides the
size of each entry in `ResultsByTime`. The forecast operation is the exception:
it supports `DAILY` and `MONTHLY` only. Metric names have to be spelled exactly
as listed below, because an unknown name is a validation error rather than an
empty result.

| Metric | What it returns |
| --- | --- |
| `BlendedCost` | Cost after a shared cost is split across accounts |
| `UnblendedCost` | Cost before a shared cost is split, before discounts |
| `AmortizedCost` | Upfront and recurring fees spread over their months |
| `NetUnblendedCost` | `UnblendedCost` after discounts |
| `NetAmortizedCost` | `AmortizedCost` after discounts |
| `NormalizedUsageAmount` | Usage in comparable units across unlike sizes |
| `UsageQuantity` | The raw count, meaningless once units are mixed |
