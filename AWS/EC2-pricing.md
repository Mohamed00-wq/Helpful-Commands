# 💰 EC2 Pricing Models

> Essential EC2 pricing CLI commands for Spot, Reserved Instances, Savings Plans, and On-Demand — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Spot Instances

| Command | Description |
|---|---|
| `aws ec2 describe-spot-price-history --instance-types t2.micro --product-descriptions "Linux/UNIX" --start-time $(date -u +"%Y-%m-%dT%H:%M:%SZ")` | View current Spot price history |
| `aws ec2 describe-spot-price-history --instance-types t2.micro --product-descriptions "Linux/UNIX" --start-time 2026-01-01T00:00:00Z --end-time 2026-01-02T00:00:00Z` | View Spot price history for a date range |
| `aws ec2 request-spot-instances --spot-price "0.005" --instance-count 1 --type "one-time" --launch-specification file://spec.json` | Request Spot instances via JSON spec |
| `aws ec2 describe-spot-instance-requests` | List all Spot instance requests |
| `aws ec2 describe-spot-instance-requests --filters "Name=state,Values=active"` | List active Spot requests only |
| `aws ec2 cancel-spot-instance-requests --spot-instance-request-ids sir-xxxxxxxx` | Cancel a Spot instance request |
| `aws ec2 describe-spot-data-feeds` | Describe the Spot data feed subscription |

```bash
# Spot price history
aws ec2 describe-spot-price-history \
  --instance-types t2.micro \
  --product-descriptions "Linux/UNIX" \
  --start-time $(date -u +"%Y-%m-%dT%H:%M:%SZ")

# Request Spot instances
aws ec2 request-spot-instances \
  --spot-price "0.005" \
  --instance-count 1 \
  --type "one-time" \
  --launch-specification file://spec.json

# Manage Spot requests
aws ec2 describe-spot-instance-requests
aws ec2 cancel-spot-instance-requests --spot-instance-request-ids sir-xxxxxxxx
```

---

## Reserved Instances

| Command | Description |
|---|---|
| `aws ec2 describe-reserved-instances` | List all Reserved Instances |
| `aws ec2 describe-reserved-instances --filters "Name=state,Values=active"` | List active RIs only |
| `aws ec2 describe-reserved-instances-offerings --instance-type t2.micro --product-description "Linux/UNIX" --term-duration P1Y` | Browse 1-year RI offerings |
| `aws ec2 describe-reserved-instances-offerings --instance-type t2.micro --product-description "Linux/UNIX" --term-duration P3Y` | Browse 3-year RI offerings |
| `aws ec2 describe-reserved-instances-offerings --instance-type t2.micro --product-description "Linux/UNIX" --offering-class standard` | Browse standard RIs |
| `aws ec2 describe-reserved-instances-offerings --instance-type t2.micro --product-description "Linux/UNIX" --offering-class convertible` | Browse convertible RIs |
| `aws ec2 purchase-reserved-instances-offering --reserved-instances-offering-id xxx --instance-count 1` | Purchase a Reserved Instance |

```bash
# List RIs
aws ec2 describe-reserved-instances --filters "Name=state,Values=active"

# Browse offerings
aws ec2 describe-reserved-instances-offerings \
  --instance-type t2.micro \
  --product-description "Linux/UNIX" \
  --term-duration P1Y
aws ec2 describe-reserved-instances-offerings \
  --instance-type t2.micro \
  --product-description "Linux/UNIX" \
  --term-duration P3Y

# Purchase
aws ec2 purchase-reserved-instances-offering \
  --reserved-instances-offering-id xxx \
  --instance-count 1
```

---

## Savings Plans

| Command | Description |
|---|---|
| `aws ec2 describe-savings-plans` | List all active Savings Plans |
| `aws ec2 describe-savings-plans --filters "Name=state,Values=active"` | List active plans only |
| `aws savingsplans describe-savings-plans-utilization --time-period-start 2026-01-01T00:00:00Z --time-period-end 2026-01-31T00:00:00Z --granularity monthly` | Check Savings Plans utilization |

```bash
aws ec2 describe-savings-plans
aws savingsplans describe-savings-plans-utilization \
  --time-period-start 2026-01-01T00:00:00Z \
  --time-period-end 2026-01-31T00:00:00Z \
  --granularity monthly
```

---

## On-Demand Pricing (Pricing API)

| Command | Description |
|---|---|
| `aws pricing get-products --service-code AmazonEC2 --filters "Name=instanceType,Values=t2.micro" "Name=location,Values=US East (N. Virginia)" --region us-east-1` | Query On-Demand pricing for a specific instance type |
| `aws pricing get-products --service-code AmazonEC2 --filters "Name=tenancy,Values=Shared" "Name=operatingSystem,Values=Linux" "Name=instanceType,Values=t3.micro" --region us-east-1` | Query shared-tenancy Linux pricing |
| `aws ec2 describe-conversion-tasks` | List instance conversion tasks |

```bash
# Query On-Demand pricing
aws pricing get-products \
  --service-code AmazonEC2 \
  --filters "Name=instanceType,Values=t2.micro" \
            "Name=location,Values=US East (N. Virginia)" \
  --region us-east-1

aws pricing get-products \
  --service-code AmazonEC2 \
  --filters "Name=tenancy,Values=Shared" \
            "Name=operatingSystem,Values=Linux" \
            "Name=instanceType,Values=t3.micro" \
  --region us-east-1
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
