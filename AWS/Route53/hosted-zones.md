# Hosted Zones

> Hosted Zones. Part of the [Route53](../) cheatsheet.

## To list all hosted zones

The following example lists all hosted zones.

```bash
aws route53 list-hosted-zones
```

## To get hosted zone details

The following example gets hosted zone details.

```bash
aws route53 get-hosted-zone --id Z1234567890ABC
```

## To create a hosted zone

The following example creates a hosted zone.

```bash
aws route53 create-hosted-zone --name example.com --caller-reference $(date +%s)
```

## To delete a hosted zone

The following example deletes a hosted zone.

```bash
aws route53 delete-hosted-zone --id Z1234567890ABC
```

## To list all records in a zone

The following example lists all records in a zone.

```bash
aws route53 list-resource-record-sets --hosted-zone-id Z1234567890ABC
```
