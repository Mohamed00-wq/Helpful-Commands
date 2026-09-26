# Cost Explorer (AWS Cost Explorer)

> Commands for cost and usage, forecasts, dimension values, tags, and Savings
> Plans recommendations.

The CLI command is `aws ce`; neither `aws costexplorer` nor
`aws cost-explorer` is a service name the CLI accepts. The operation is
`get-cost-and-usage`, and `get-cost-usage` is a typo that the CLI rejects with
its list of valid operations. Every call needs `--time-period`, `--granularity`,
and at least one value in `--metrics`.

Most of the errors people hit here come down to a few details. The data lands
with roughly a 24 hour delay, so today and yesterday are always incomplete. The
date format is strictly `YYYY-MM-DD` with no time component, and `End` is
exclusive, so a whole month is `Start=2026-01-01,End=2026-02-01`. And the
documented Cost Explorer API endpoint is `ce.us-east-1.amazonaws.com`, so pass
`--region us-east-1` when your profile defaults anywhere else. Cost Explorer
also holds only the current month plus the previous 13 months.

## Cost and Usage

### To get the cost of a month

The following example returns the blended cost of one month.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics BlendedCost
```

### To get the cost of the last 30 days

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

### To get the cost of one service

The following example filters on the `SERVICE` dimension, and the value is the
service name Cost Explorer uses rather than the acronym.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'Dimensions={Key=SERVICE,Values=AmazonEC2}'
```

### To get the cost of one tag value

The following example filters on a cost allocation tag, and the tag has to be
active in the account before its costs can be filtered on.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'Tags={Key=Environment,Values=prod}'
```

### To get the cost of one account in an organization

The following example filters on the `LINKED_ACCOUNT` dimension, which is how
you get one member account out of a payer view.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'Dimensions={Key=LINKED_ACCOUNT,Values=<account-id>}'
```

### To combine two filters

The following example intersects a service filter with a tag filter, and the
brace nesting is what trips people up when they first meet the shorthand.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --filter 'And=[{Dimensions={Key=SERVICE,Values=AmazonEC2}},{Tags={Key=Environment,Values=prod}}]'
```

### To pass a filter from a file

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

### To return several metrics in one call

The following example asks for three cost metrics at once, and the response then
carries a block per metric under `Total` and under each group.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics BlendedCost UnblendedCost AmortizedCost
```

### To read the total as a plain number

The following example prints the month total as text, which is what you want
when the value is going into a script or a budget check.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics BlendedCost \
    --query 'ResultsByTime[0].Total.BlendedCost.Amount' --output text
```

### To page through the results

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

## Cost by Dimension

Grouping splits the cost into one bucket per value of the dimension, and each
bucket appears under `Groups` in every time bucket of the response.

### To get the cost of every service

The following example groups the month by service.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=SERVICE
```

### To get the cost of every Region

The following example groups the month by Region.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=REGION
```

### To get the cost by usage type

The following example groups the month by usage type, which is the level to drop
to when a service line item is too coarse to explain.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=USAGE_TYPE
```

### To get the cost of every value of one tag

The following example groups by tag, and a tag bucket only appears for resources
that carry the tag, so the buckets never add up to the account total.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=TAG,Key=Environment
```

### To get the cost of a service and a Region together

The following example adds both groupings, and each bucket then carries the
service in `Keys[0]` and the Region in `Keys[1]`.

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=SERVICE Type=DIMENSION,Key=REGION
```

## Granularity and Metrics

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

## Forecasts

### To forecast a month's cost

The following example forecasts one month from the usage recorded in it.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY
```

### To forecast with a confidence band

The following example asks for an 80 percent prediction interval, which is what
you want before you commit a budget number rather than the mean alone.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY \
    --prediction-interval-level 80
```

### To read the forecast mean

The following example prints the mean forecast value as text, and the buckets
sit under `ForecastResultsByTime` rather than under a `ForecastResults` key.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY \
    --query 'ForecastResultsByTime[0].MeanValue' --output text
```

### To forecast the cost of one service

The following example narrows the forecast to a single service, and the metric
name here is upper case with underscores, unlike the `--metrics` values above.

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --metric BLENDED_COST \
    --granularity MONTHLY \
    --filter 'Dimensions={Key=SERVICE,Values=AmazonEC2}'
```

## Dimensions

### To list the service names

The following example returns the service names you can filter and group on,
which is the quickest way to get the exact spelling of a value.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension SERVICE \
    --search-string 'Amazon'
```

### To list the usage types of a service

The following example returns the usage types Cost Explorer bills a service
under, and these strings are what a `USAGE_TYPE` filter takes.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension USAGE_TYPE \
    --search-string 'BoxUsage'
```

### To list the usage types of a Reserved Instance

The following example switches the context to reservations, which is what
returns the normalized usage types a Reserved Instance covers.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension USAGE_TYPE \
    --context RESERVATIONS \
    --search-string 'BoxUsage'
```

### To page through dimension values

The following example caps a page at 20 values, and the `NextPageToken` from the
response is what `--next-page-token` takes to reach the next page.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension SERVICE \
    --max-results 20
```

## Tags

`get-tags` returns the tag keys and nothing else, so it answers which keys are
active and never what their values are. Only keys switched on in the billing
console appear, and a key activated part way through a month has no history
before that point.

### To list the cost allocation tags

The following example returns the tag keys the account reports on.

```bash
aws ce get-tags \
    --time-period Start=2026-01-01,End=2026-02-01
```

### To list one tag key

The following example returns a single tag key, and `--search-string` filters
the key list the same way.

```bash
aws ce get-tags \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --tag-key Environment
```

### To list the tag values in use

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

## Savings Plans Recommendations

The recommendation is calculated from your recent usage, and a consolidated
billing family gets only three refresh requests per 24 hours, so a retry loop
against this operation runs out of quota quickly.

### To get a Compute Savings Plans recommendation

The following example returns the recommended commitment for a one year No
Upfront Compute Savings Plan over the last 30 days of usage.

```bash
aws ce get-savings-plans-purchase-recommendation \
    --savings-plans-type COMPUTE_SP \
    --term-in-years ONE_YEAR \
    --payment-option NO_UPFRONT \
    --lookback-period-in-days THIRTY_DAYS
```

### To get a recommendation for the whole billing family

The following example asks for a three year Compute plan across the payer and
its member accounts rather than for one account alone.

```bash
aws ce get-savings-plans-purchase-recommendation \
    --savings-plans-type COMPUTE_SP \
    --term-in-years THREE_YEARS \
    --payment-option ALL_UPFRONT \
    --account-scope PAYER \
    --lookback-period-in-days THIRTY_DAYS
```

### To get an EC2 Instance Savings Plans recommendation

The following example returns the narrower plan type, which only covers instance
sizes in the same family.

```bash
aws ce get-savings-plans-purchase-recommendation \
    --savings-plans-type EC2_INSTANCE_SP \
    --term-in-years ONE_YEAR \
    --payment-option NO_UPFRONT \
    --lookback-period-in-days THIRTY_DAYS
```

### To read the recommended hourly commitment

The following example prints the recommended hourly commitment as text, and the
summary block is what holds the totals across the per Region details.

```bash
aws ce get-savings-plans-purchase-recommendation \
    --savings-plans-type COMPUTE_SP \
    --term-in-years ONE_YEAR \
    --payment-option NO_UPFRONT \
    --lookback-period-in-days THIRTY_DAYS \
    --query 'SavingsPlansPurchaseRecommendation.SavingsPlansPurchaseRecommendationSummary.HourlyCommitmentToPurchase' \
    --output text
```

## Pricing

The Price List Query API is the list price side of billing, and it is the only
way to get a price before you buy anything. It is not Cost Explorer: the service
is `aws pricing`, and it is only served from `us-east-1`, `ap-south-1`, and
`eu-central-1`, so a query against any other Region fails.

### To look up a list price

The following example returns the SKU level price for one instance type, and the
`location` attribute is the human readable Region name rather than the Region
code.

```bash
aws pricing get-products \
    --service-code AmazonEC2 \
    --region us-east-1 \
    --filters 'Type=TERM_MATCH,Field=instanceType,Value=m5.large' \
    'Type=TERM_MATCH,Field=operatingSystem,Value=Linux' \
    --max-results 5
```

### To list the service codes

The following example returns every service code, which is the value
`--service-code` takes.

```bash
aws pricing describe-services --region us-east-1 --max-results 25
```

## Workflows

### To build a month-to-date summary by service

1. Work out the time period from `date` and read the account total. The `date`
   commands return the first day of the current and previous months, so the
   window always lands on full month boundaries.

   ```bash
   START=$(date -u -d "$(date -u +%Y-%m-01) -1 month" +%Y-%m-01)
   END=$(date -u +%Y-%m-01)
   echo "$START to $END"
   ```

2. Read the total for the period and keep it, because the group buckets in the
   next step do not add up to it.

   ```bash
   TOTAL=$(aws ce get-cost-and-usage \
       --time-period "Start=$START,End=$END" \
       --granularity MONTHLY \
       --metrics UnblendedCost \
       --query 'ResultsByTime[0].Total.UnblendedCost.Amount' --output text)
   echo "$TOTAL"
   ```

3. Break the period down by service and print the biggest spenders first.

   ```bash
   aws ce get-cost-and-usage \
       --time-period "Start=$START,End=$END" \
       --granularity MONTHLY \
       --metrics UnblendedCost \
       --group-by Type=DIMENSION,Key=SERVICE \
       --query 'reverse(sort_by(ResultsByTime[0].Groups,& Metrics.UnblendedCost.Amount))[].[Keys[0],Metrics.UnblendedCost.Amount]' \
       --output table
   ```

4. Recurse into the service that surprises you, one usage type at a time.

   ```bash
   aws ce get-cost-and-usage \
       --time-period "Start=$START,End=$END" \
       --granularity MONTHLY \
       --metrics UnblendedCost \
       --group-by Type=DIMENSION,Key=USAGE_TYPE \
       --filter 'Dimensions={Key=SERVICE,Values=AmazonEC2}' \
       --query 'ResultsByTime[0].Groups[].[Keys[0],Metrics.UnblendedCost.Amount]' \
       --output table
   ```

### To find spend outside one tag value

1. Confirm the tag key is active, because a key that is switched on late has no
   history to filter on.

   ```bash
   aws ce get-tags \
       --time-period Start=2026-01-01,End=2026-02-01 \
       --tag-key Environment
   ```

2. Total the cost that does not carry the tag value. Untagged resources are
   inside this number, because no tag means no match on `prod`.

   ```bash
   aws ce get-cost-and-usage \
       --time-period Start=2026-01-01,End=2026-02-01 \
       --granularity MONTHLY \
       --metrics UnblendedCost \
       --filter 'Not={Tags={Key=Environment,Values=prod}}' \
       --query 'ResultsByTime[0].Total.UnblendedCost.Amount' --output text
   ```

3. List the services responsible, so you know which teams to chase.

   ```bash
   aws ce get-cost-and-usage \
       --time-period Start=2026-01-01,End=2026-02-01 \
       --granularity MONTHLY \
       --metrics UnblendedCost \
       --group-by Type=DIMENSION,Key=SERVICE \
       --filter 'Not={Tags={Key=Environment,Values=prod}}' \
       --query 'reverse(sort_by(ResultsByTime[0].Groups,& Metrics.UnblendedCost.Amount))[].[Keys[0],Metrics.UnblendedCost.Amount]' \
       --output table
   ```
