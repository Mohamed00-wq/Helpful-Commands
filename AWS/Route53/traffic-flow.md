# Traffic Flow

> Traffic Flow. Part of the [Route53](../) cheatsheet.

## To list records

The following example lists records.

```bash
aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC --max-items 100
```

## To delete a record

The following example deletes a record.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch '{"Changes":[{"Action":"DELETE","ResourceRecordSet":{"Name":"app.example.com","Type":"A","TTL":60,"ResourceRecords":[{"Value":"1.1.1.1"}]}}]}'
```
