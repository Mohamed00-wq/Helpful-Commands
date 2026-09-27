# Domain Registration

> Domain Registration. Part of the [Route53](../Route53.md) cheatsheet.

## To list registered domains

The following example lists registered domains.

```bash
aws route53 domains list-domains
```

## To get domain details

The following example gets domain details.

```bash
aws route53 domains get-domain-detail --domain-name example.com
```

## To get domain suggestions

The following example gets domain suggestions.

```bash
aws route53 domains get-domain-suggestions --domain-name example --tld com
```

## To check availability

The following example checks availability.

```bash
aws route53 domains check-domain-availability --domain-name example.com
```

## To transfer domain to R53

The following example transfers domain to R53.

```bash
aws route53 domains transfer-domain-to-route53 --domain-name example.com --auth-code xxx
```
