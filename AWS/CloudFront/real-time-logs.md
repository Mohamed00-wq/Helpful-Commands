# Real-Time Logs

> Real-Time Logs. Part of the [CloudFront](../) cheatsheet.

## To list distribution IDs

The following example lists distribution IDs.

```bash
aws cloudfront list-distributions --query "DistributionList.Items[*].{Id:Id,DomainName:DomainName}"
```

```bash
# Real-time logs require Kinesis Data Stream + CloudWatch Logs
# See AWS docs for full setup
```
