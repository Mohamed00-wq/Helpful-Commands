# Record Sets

> Record Sets. Part of the [Route53](../Route53.md) cheatsheet.

## To create or update record sets

The following example applies a change batch that can create, update, or delete records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://change.json
```

## To find a specific record

The following example finds a specific record.

```bash
aws route53 list-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --query "ResourceRecordSets[?Name=='api.example.com.']"
```
