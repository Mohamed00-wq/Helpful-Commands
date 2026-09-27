# Health Checks

> Health Checks. Part of the [Route53](../Route53.md) cheatsheet.

## To list all health checks

The following example lists all health checks.

```bash
aws route53 list-health-checks
```

## To get health check details

The following example gets health check details.

```bash
aws route53 get-health-check --health-check-id xxx
```

## To create HTTP health check

The following example creates HTTP health check.

```bash
aws route53 create-health-check
    --caller-reference $(date +%s)
    --health-check-config '{"IPAddress":"1.2.3.4","Port":80,"Type":"HTTP","ResourcePath":"/","RequestInterval":30,"FailureThreshold":3}'
```

## To create HTTPS health check

The following example creates HTTPS health check.

```bash
aws route53 create-health-check
    --caller-reference $(date +%s)
    --health-check-config '{"FullyQualifiedDomainName":"example.com","Port":443,"Type":"HTTPS","ResourcePath":"/health","RequestInterval":10,"FailureThreshold":2}'
```

## To delete a health check

The following example deletes a health check.

```bash
aws route53 delete-health-check --health-check-id xxx
```

## To get health check status

The following example gets health check status.

```bash
aws route53 get-health-check-status --health-check-id xxx
```

## To get last failure reason

The following example gets last failure reason.

```bash
aws route53 get-health-check-last-failure-reason --health-check-id xxx
```
