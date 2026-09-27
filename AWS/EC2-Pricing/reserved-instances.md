# Reserved Instances

> Reserved Instances. Part of the [EC2-Pricing](../EC2-Pricing.md) cheatsheet.

## To list all Reserved Instances

## To list active RIs only

The following example lists active RIs only.

```bash
aws ec2 describe-reserved-instances --filters "Name=state,Values=active"
```

## To browse 1-year RI offerings

The following example browses 1-year RI offerings.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --term-duration P1Y
```

## To browse 3-year RI offerings

The following example browses 3-year RI offerings.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --term-duration P3Y
```

## To browse standard RIs

The following example browses standard RIs.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --offering-class standard
```

## To browse convertible RIs

The following example browses convertible RIs.

```bash
aws ec2 describe-reserved-instances-offerings
    --instance-type t3.micro
    --product-description "Linux/UNIX"
    --offering-class convertible
```

## To purchase a Reserved Instance

The following example purchases a Reserved Instance.

```bash
aws ec2 purchase-reserved-instances-offering --reserved-instances-offering-id xxx --instance-count 1
```
