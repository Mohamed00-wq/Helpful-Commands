# Savings Plans Recommendations

> Savings Plans Recommendations. Part of the [CostExplorer](../CostExplorer.md) cheatsheet.

The recommendation is calculated from your recent usage, and a consolidated
billing family gets only three refresh requests per 24 hours, so a retry loop
against this operation runs out of quota quickly.

## To get a Compute Savings Plans recommendation

The following example returns the recommended commitment for a one year No
Upfront Compute Savings Plan over the last 30 days of usage.

```bash
aws ce get-savings-plans-purchase-recommendation \
    --savings-plans-type COMPUTE_SP \
    --term-in-years ONE_YEAR \
    --payment-option NO_UPFRONT \
    --lookback-period-in-days THIRTY_DAYS
```

## To get a recommendation for the whole billing family

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

## To get an EC2 Instance Savings Plans recommendation

The following example returns the narrower plan type, which only covers instance
sizes in the same family.

```bash
aws ce get-savings-plans-purchase-recommendation \
    --savings-plans-type EC2_INSTANCE_SP \
    --term-in-years ONE_YEAR \
    --payment-option NO_UPFRONT \
    --lookback-period-in-days THIRTY_DAYS
```

## To read the recommended hourly commitment

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
