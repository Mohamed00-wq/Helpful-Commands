# Alias Records (ALB/NLB/CloudFront)

> Alias Records (ALB/NLB/CloudFront). Part of the [Route53](../) cheatsheet.

## To create alias record

The following example creates alias record.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://alias.json
```
