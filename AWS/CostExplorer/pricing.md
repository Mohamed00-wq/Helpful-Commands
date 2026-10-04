# Pricing

> Pricing. Part of the [CostExplorer](../) cheatsheet.

The Price List Query API is the list price side of billing, and it is the only
way to get a price before you buy anything. It is not Cost Explorer: the service
is `aws pricing`, and it is only served from `us-east-1`, `ap-south-1`, and
`eu-central-1`, so a query against any other Region fails.

## To look up a list price

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

## To list the service codes

The following example returns every service code, which is the value
`--service-code` takes.

```bash
aws pricing describe-services --region us-east-1 --max-results 25
```
