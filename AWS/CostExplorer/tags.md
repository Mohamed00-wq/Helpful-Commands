# Tags

> Tags. Part of the [CostExplorer](../) cheatsheet.

`get-tags` returns the tag keys and nothing else, so it answers which keys are
active and never what their values are. Only keys switched on in the billing
console appear, and a key activated part way through a month has no history
before that point.

## To list the cost allocation tags

The following example returns the tag keys the account reports on.

```bash
aws ce get-tags \
    --time-period Start=2026-01-01,End=2026-02-01
```

## To list one tag key

The following example returns a single tag key, and `--search-string` filters
the key list the same way.

```bash
aws ce get-tags \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --tag-key Environment
```

## To list the tag values in use

The following example groups the month on the tag to get its values, because
`get-tags` returns the keys alone.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=TAG,Key=Environment \
    --query 'ResultsByTime[0].Groups[].Keys[0]' --output text
```
