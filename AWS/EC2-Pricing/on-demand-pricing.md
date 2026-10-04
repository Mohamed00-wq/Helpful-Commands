# On-Demand Pricing (Pricing API)

> On-Demand Pricing (Pricing API). Part of the [EC2-Pricing](../) cheatsheet.

## To query On-Demand pricing for a specific instance type

The following example queries On-Demand pricing for a specific instance type.

```bash
aws pricing get-products
    --service-code AmazonEC2
    --filters "Name=instanceType,Values=t3.micro" "Name=location,Values=US East (N. Virginia)"
    --region us-east-1
```

## To query shared-tenancy Linux pricing

The following example queries shared-tenancy Linux pricing.

```bash
aws pricing get-products
    --service-code AmazonEC2
    --filters "Name=tenancy,Values=Shared" "Name=operatingSystem,Values=Linux" "Name=instanceType,Values=t3.micro"
    --region us-east-1
```

## To list instance conversion tasks

The following example lists instance conversion tasks.

```bash
aws ec2 describe-conversion-tasks
```
