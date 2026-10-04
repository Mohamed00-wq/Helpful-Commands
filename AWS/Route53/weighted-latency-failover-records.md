# Weighted / Latency / Failover Records

> Weighted / Latency / Failover Records. Part of the [Route53](../) cheatsheet.

## To create weighted records

The following example creates weighted records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://weighted.json
```

## To create latency records

The following example creates latency records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://latency.json
```

## To create failover records

The following example creates failover records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://failover.json
```

```bash
# Weighted:
# {"Changes":[{"Action":"CREATE","ResourceRecordSet":{
#   "Name":"app.example.com","Type":"A","SetIdentifier":"us-east-1",
#   "Weight":70,"TTL":60,
#   "ResourceRecords":[{"Value":"1.1.1.1"}]}}]}

# Latency:
# {"Changes":[{"Action":"CREATE","ResourceRecordSet":{
#   "Name":"app.example.com","Type":"A","SetIdentifier":"us-east-1",
#   "Region":"us-east-1","TTL":60,
#   "ResourceRecords":[{"Value":"1.1.1.1"}]}}]}
```
