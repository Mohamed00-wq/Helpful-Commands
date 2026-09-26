# Route 53 (DNS and Domain Registration)

> Commands for hosted zones, record sets, health checks, and routing policies.

## Hosted Zones

### To list all hosted zones

The following example lists all hosted zones.

```bash
aws route53 list-hosted-zones
```

### To get hosted zone details

The following example gets hosted zone details.

```bash
aws route53 get-hosted-zone --id Z1234567890ABC
```

### To create a hosted zone

The following example creates a hosted zone.

```bash
aws route53 create-hosted-zone --name example.com --caller-reference $(date +%s)
```

### To delete a hosted zone

The following example deletes a hosted zone.

```bash
aws route53 delete-hosted-zone --id Z1234567890ABC
```

### To list all records in a zone

The following example lists all records in a zone.

```bash
aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC
```

## Record Sets

### To create or update record sets

The following example applies a change batch that can create, update, or delete records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://change.json
```

### To find a specific record

The following example finds a specific record.

```bash
aws route53 list-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --query "ResourceRecordSets[?Name=='api.example.com.']"
```

## Alias Records (ALB/NLB/CloudFront)

### To create alias record

The following example creates alias record.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://alias.json
```

## Weighted / Latency / Failover Records

### To create weighted records

The following example creates weighted records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://weighted.json
```

### To create latency records

The following example creates latency records.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch file://latency.json
```

### To create failover records

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

## Health Checks

### To list all health checks

The following example lists all health checks.

```bash
aws route53 list-health-checks
```

### To get health check details

The following example gets health check details.

```bash
aws route53 get-health-check --health-check-id xxx
```

### To create HTTP health check

The following example creates HTTP health check.

```bash
aws route53 create-health-check
    --caller-reference $(date +%s)
    --health-check-config '{"IPAddress":"1.2.3.4","Port":80,"Type":"HTTP","ResourcePath":"/","RequestInterval":30,"FailureThreshold":3}'
```

### To create HTTPS health check

The following example creates HTTPS health check.

```bash
aws route53 create-health-check
    --caller-reference $(date +%s)
    --health-check-config '{"FullyQualifiedDomainName":"example.com","Port":443,"Type":"HTTPS","ResourcePath":"/health","RequestInterval":10,"FailureThreshold":2}'
```

### To delete a health check

The following example deletes a health check.

```bash
aws route53 delete-health-check --health-check-id xxx
```

### To get health check status

The following example gets health check status.

```bash
aws route53 get-health-check-status --health-check-id xxx
```

### To get last failure reason

The following example gets last failure reason.

```bash
aws route53 get-health-check-last-failure-reason --health-check-id xxx
```

## Traffic Flow

### To list records

The following example lists records.

```bash
aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC --max-items 100
```

### To delete a record

The following example deletes a record.

```bash
aws route53 change-resource-record-sets
    --hosted-zone-id Z1234567890ABC
    --change-batch '{"Changes":[{"Action":"DELETE","ResourceRecordSet":{"Name":"app.example.com","Type":"A","TTL":60,"ResourceRecords":[{"Value":"1.1.1.1"}]}}]}'
```

## Domain Registration

### To list registered domains

The following example lists registered domains.

```bash
aws route53 domains list-domains
```

### To get domain details

The following example gets domain details.

```bash
aws route53 domains get-domain-detail --domain-name example.com
```

### To get domain suggestions

The following example gets domain suggestions.

```bash
aws route53 domains get-domain-suggestions --domain-name example --tld com
```

### To check availability

The following example checks availability.

```bash
aws route53 domains check-domain-availability --domain-name example.com
```

### To transfer domain to R53

The following example transfers domain to R53.

```bash
aws route53 domains transfer-domain-to-route53 --domain-name example.com --auth-code xxx
```
