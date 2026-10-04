# Savings Plans

> Savings Plans. Part of the [EC2-Pricing](../) cheatsheet.

## To list all active Savings Plans

## To list active plans only

The following example lists active plans only.

```bash
aws ec2 describe-savings-plans --filters "Name=state,Values=active"
```

## To check Savings Plans utilization

The following example checks Savings Plans utilization.

```bash
aws savingsplans describe-savings-plans-utilization
    --time-period-start 2026-01-01T00:00:00Z
    --time-period-end 2026-01-31T00:00:00Z
    --granularity monthly
```
