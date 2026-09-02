# 🌍 Route 53 (DNS & Domain Registration)

> Advanced Route 53 CLI commands for hosted zones, records, health checks, and domain registration — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Hosted Zones

| Command | Description |
|---|---|
| `aws route53 list-hosted-zones` | List all hosted zones |
| `aws route53 get-hosted-zone --id Z1234567890ABC` | Get hosted zone details |
| `aws route53 create-hosted-zone --name example.com --caller-reference $(date +%s)` | Create a hosted zone |
| `aws route53 delete-hosted-zone --id Z1234567890ABC` | Delete a hosted zone |
| `aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC` | List all records in a zone |

```bash
# Create hosted zone
aws route53 create-hosted-zone \
  --name example.com \
  --caller-reference $(date +%s)

# List records
aws route53 list-resource-record-sets \
  --hosted-zone-id Z1234567890ABC
```

---

## Record Sets

| Command | Description |
|---|---|
| `aws route53 change-resource-record-sets --hosted-zone-id Z1234567890ABC --change-batch file://change.json` | Create/update records |
| `aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC --query "ResourceRecordSets[?Name=='api.example.com.']"` | Find a specific record |

```bash
# change.json example:
# {
#   "Changes": [
#     {
#       "Action": "CREATE",
#       "ResourceRecordSet": {
#         "Name": "www.example.com",
#         "Type": "A",
#         "TTL": 300,
#         "ResourceRecords": [{"Value": "1.2.3.4"}]
#       }
#     }
#   ]
# }

aws route53 change-resource-record-sets \
  --hosted-zone-id Z1234567890ABC \
  --change-batch file://change.json
```

---

## Alias Records (ALB/NLB/CloudFront)

| Command | Description |
|---|---|
| `aws route53 change-resource-record-sets --hosted-zone-id Z1234567890ABC --change-batch file://alias.json` | Create alias record |

```bash
# alias.json for ALB:
# {
#   "Changes": [{
#     "Action": "CREATE",
#     "ResourceRecordSet": {
#       "Name": "app.example.com",
#       "Type": "A",
#       "AliasTarget": {
#         "HostedZoneId": "Z35SXDOTRQ7X7K",
#         "DNSName": "my-alb-123456.us-east-1.elb.amazonaws.com",
#         "EvaluateTargetHealth": true
#       }
#     }
#   }]
# }

aws route53 change-resource-record-sets \
  --hosted-zone-id Z1234567890ABC \
  --change-batch file://alias.json
```

---

## Weighted / Latency / Failover Records

| Command | Description |
|---|---|
| `aws route53 change-resource-record-sets --hosted-zone-id Z1234567890ABC --change-batch file://weighted.json` | Create weighted records |
| `aws route53 change-resource-record-sets --hosted-zone-id Z1234567890ABC --change-batch file://latency.json` | Create latency records |
| `aws route53 change-resource-record-sets --hosted-zone-id Z1234567890ABC --change-batch file://failover.json` | Create failover records |

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

---

## Health Checks

| Command | Description |
|---|---|
| `aws route53 list-health-checks` | List all health checks |
| `aws route53 get-health-check --health-check-id xxx` | Get health check details |
| `aws route53 create-health-check --caller-reference $(date +%s) --health-check-config '{"IPAddress":"1.2.3.4","Port":80,"Type":"HTTP","ResourcePath":"/","RequestInterval":30,"FailureThreshold":3}'` | Create HTTP health check |
| `aws route53 create-health-check --caller-reference $(date +%s) --health-check-config '{"FullyQualifiedDomainName":"example.com","Port":443,"Type":"HTTPS","ResourcePath":"/health","RequestInterval":10,"FailureThreshold":2}'` | Create HTTPS health check |
| `aws route53 delete-health-check --health-check-id xxx` | Delete a health check |
| `aws route53 get-health-check-status --health-check-id xxx` | Get health check status |
| `aws route53 get-health-check-last-failure-reason --health-check-id xxx` | Get last failure reason |

```bash
# Create health check
aws route53 create-health-check \
  --caller-reference $(date +%s) \
  --health-check-config '{"IPAddress":"1.2.3.4","Port":80,"Type":"HTTP","ResourcePath":"/","RequestInterval":30,"FailureThreshold":3}'
```

---

## Traffic Flow

| Command | Description |
|---|---|
| `aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC --max-items 100` | List records |
| `aws route53 change-resource-record-sets --hosted-zone-id Z1234567890ABC --change-batch '{"Changes":[{"Action":"DELETE","ResourceRecordSet":{"Name":"app.example.com","Type":"A","TTL":60,"ResourceRecords":[{"Value":"1.1.1.1"}]}}]}'` | Delete a record |

```bash
# Delete a record
aws route53 change-resource-record-sets \
  --hosted-zone-id Z1234567890ABC \
  --change-batch '{"Changes":[{"Action":"DELETE","ResourceRecordSet":{"Name":"app.example.com","Type":"A","TTL":60,"ResourceRecords":[{"Value":"1.1.1.1"}]}}]}'
```

---

## Domain Registration

| Command | Description |
|---|---|
| `aws route53 domains list-domains` | List registered domains |
| `aws route53 domains get-domain-detail --domain-name example.com` | Get domain details |
| `aws route53 domains get-domain-suggestions --domain-name example --tld com` | Get domain suggestions |
| `aws route53 domains check-domain-availability --domain-name example.com` | Check availability |
| `aws route53 domains transfer-domain-to-route53 --domain-name example.com --auth-code xxx` | Transfer domain to R53 |

```bash
# Check availability
aws route53 domains check-domain-availability --domain-name example.com
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
