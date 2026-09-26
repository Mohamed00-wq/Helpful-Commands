# EC2 Pricing (On-Demand, Spot, Reserved, Savings Plans)

> Commands for comparing On-Demand, Spot, Reserved Instance, and Savings Plan pricing.

## Spot Instances

### To view current Spot price history

The following example views current Spot price history.

```bash
aws ec2 describe-spot-price-history
    --instance-types t3.micro
    --product-descriptions "Linux/UNIX"
    --start-time $(date
    -u +"%Y-%m-%dT%H:%M:%SZ")
```

### To view Spot price history for a date range

The following example views Spot price history for a date range.

```bash
aws ec2 describe-spot-price-history
    --instance-types t3.micro
    --product-descriptions "Linux/UNIX"
    --start-time 2026-01-01T00:00:00Z
    --end-time 2026-01-02T00:00:00Z
```

### To request Spot instances via JSON spec

The following example requests Spot instances via JSON spec.

```bash
aws ec2 request-spot-instances
    --spot-price "0.005"
    --instance-count 1
    --type "one-time"
    --launch-specification file://spec.json
```

### To list all Spot instance requests

### To list active Spot requests only

The following example lists active Spot requests only.

```bash
aws ec2 describe-spot-instance-requests --filters "Name=state,Values=active"
```

### To cancel a Spot instance request

The following example cancels a Spot instance request.

```bash
aws ec2 cancel-spot-instance-requests --spot-instance-request-ids sir-xxxxxxxx
```

### To describe the Spot data feed subscription

The following example describes the Spot data feed subscription.

```bash
aws ec2 describe-spot-data-feeds
```

## Reserved Instances

### To list all Reserved Instances

### To list active RIs only

The following example lists active RIs only.

```bash
aws ec2 describe-reserved-instances --filters "Name=state,Values=active"
```

### To browse 1-year RI offerings

The following example browses 1-year RI offerings.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --term-duration P1Y
```

### To browse 3-year RI offerings

The following example browses 3-year RI offerings.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --term-duration P3Y
```

### To browse standard RIs

The following example browses standard RIs.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --offering-class standard
```

### To browse convertible RIs

The following example browses convertible RIs.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --offering-class convertible
```

### To purchase a Reserved Instance

The following example purchases a Reserved Instance.

```bash
aws ec2 purchase-reserved-instances-offering --reserved-instances-offering-id xxx --instance-count 1
```

## Savings Plans

### To list all active Savings Plans

### To list active plans only

The following example lists active plans only.

```bash
aws ec2 describe-savings-plans --filters "Name=state,Values=active"
```

### To check Savings Plans utilization

The following example checks Savings Plans utilization.

```bash
aws savingsplans describe-savings-plans-utilization
    --time-period-start 2026-01-01T00:00:00Z
    --time-period-end 2026-01-31T00:00:00Z
    --granularity monthly
```

## On-Demand Pricing (Pricing API)

### To query On-Demand pricing for a specific instance type

The following example queries On-Demand pricing for a specific instance type.

```bash
aws pricing get-products
    --service-code AmazonEC2
    --filters "Name=instanceType,Values=t3.micro" "Name=location,Values=US East (N. Virginia)"
    --region us-east-1
```

### To query shared-tenancy Linux pricing

The following example queries shared-tenancy Linux pricing.

```bash
aws pricing get-products
    --service-code AmazonEC2
    --filters "Name=tenancy,Values=Shared" "Name=operatingSystem,Values=Linux" "Name=instanceType,Values=t3.micro"
    --region us-east-1
```

### To list instance conversion tasks

The following example lists instance conversion tasks.

```bash
aws ec2 describe-conversion-tasks
```
