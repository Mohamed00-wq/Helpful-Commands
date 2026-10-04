# Spot Instances

> Spot Instances. Part of the [EC2-Pricing](../) cheatsheet.

## To view current Spot price history

The following example views current Spot price history.

```bash
aws ec2 describe-spot-price-history
    --instance-types t3.micro
    --product-descriptions "Linux/UNIX"
    --start-time $(date
    -u +"%Y-%m-%dT%H:%M:%SZ")
```

## To view Spot price history for a date range

The following example views Spot price history for a date range.

```bash
aws ec2 describe-spot-price-history
    --instance-types t3.micro
    --product-descriptions "Linux/UNIX"
    --start-time 2026-01-01T00:00:00Z
    --end-time 2026-01-02T00:00:00Z
```

## To request Spot instances via JSON spec

The following example requests Spot instances via JSON spec.

```bash
aws ec2 request-spot-instances
    --spot-price "0.005"
    --instance-count 1
    --type "one-time"
    --launch-specification file://spec.json
```

## To list all Spot instance requests

## To list active Spot requests only

The following example lists active Spot requests only.

```bash
aws ec2 describe-spot-instance-requests --filters "Name=state,Values=active"
```

## To cancel a Spot instance request

The following example cancels a Spot instance request.

```bash
aws ec2 cancel-spot-instance-requests --spot-instance-request-ids sir-xxxxxxxx
```

## To describe the Spot data feed subscription

The following example describes the Spot data feed subscription.

```bash
aws ec2 describe-spot-data-feeds
```
