# Cost and Usage

> Cost and Usage. Part of the [CostExplorer](../CostExplorer.md) cheatsheet.

## To get the cost of a month

The following example returns the blended cost of one month.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics BlendedCost
```

## To get the cost of the last 30 days

The following example builds the time period from `date`, which avoids the
hard coded date that quietly goes stale in a script.

```bash
START=$(date -u -d '30 days ago' +%Y-%m-%d)
END=$(date -u +%Y-%m-%d)
aws ce get-cost-and-usage \
    --time-period "Start=$START,End=$END" \
    --granularity DAILY \
    --metrics UnblendedCost
```

## To get the cost of one service

The following example filters on the `SERVICE` dimension, and the value is the
service name Cost Explorer uses rather than the acronym.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'Dimensions={Key=SERVICE,Values=AmazonEC2}'
```

## To get the cost of one tag value

The following example filters on a cost allocation tag, and the tag has to be
active in the account before its costs can be filtered on.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'Tags={Key=Environment,Values=prod}'
```

## To get the cost of one account in an organization

The following example filters on the `LINKED_ACCOUNT` dimension, which is how
you get one member account out of a payer view.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'Dimensions={Key=LINKED_ACCOUNT,Values=<account-id>}'
```

## To combine two filters

The following example intersects a service filter with a tag filter, and the
brace nesting is what trips people up when they first meet the shorthand.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'And=[{Dimensions={Key=SERVICE,Values=AmazonEC2}},{Tags={Key=Environment,Values=prod}}]'
```

## To pass a filter from a file

The following example reads the filter from a file, which is easier to read than
the shorthand once the expression nests more than two levels.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter file://filter.json
```

`filter.json`:

```json
{
  "And": [
    {"Dimensions": {"Key": "SERVICE", "Values": ["AmazonEC2"]}},
    {"Tags": {"Key": "Environment", "Values": ["prod"]}}
  ]
}
```

## To return several metrics in one call

The following example asks for three cost metrics at once, and the response then
carries a block per metric under `Total` and under each group.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics BlendedCost UnblendedCost AmortizedCost
```

## To read the total as a plain number

The following example prints the month total as text, which is what you want
when the value is going into a script or a budget check.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics BlendedCost \
    --query 'ResultsByTime[0].Total.BlendedCost.Amount' --output text
```

## To page through the results

The following example follows one page with the returned token. Cost Explorer
has no CLI paginator, so `--max-items` is rejected and you pass the
`NextPageToken` from the previous response by hand.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity DAILY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=USAGE_TYPE \
    --next-page-token <next-page-token>
```
