# Domain Names and VPC Links

> Domain Names and VPC Links. Part of the [API-Gateway](../) cheatsheet.

A custom domain replaces the generated `execute-api` hostname, and a VPC link
lets a private integration reach a load balancer inside a VPC.

## To create a custom domain name

The following example creates a regional custom domain with an ACM
certificate. The certificate has to be in the same region as the API.

```bash
aws apigateway create-domain-name \
    --domain-name api.example.com \
    --certificate-arn arn:aws:acm:us-east-1:<account-id>:certificate/<certificate-id> \
    --security-policy TLS_1_2
```

## To list the domain names of a region

The following example returns every domain name and its target.

```bash
aws apigateway get-domain-names
```

## To serve a stage from the root of a domain

The following example maps `https://api.example.com/` to `prod`. The value
`(none)` means an empty path.

```bash
aws apigateway create-base-path-mapping \
    --domain-name api.example.com \
    --rest-api-id <rest-api-id> \
    --stage prod \
    --base-path '(none)'
```

## To serve a stage from a path prefix

The following example maps `https://api.example.com/orders` to `prod`, so
several APIs can share one domain.

```bash
aws apigateway create-base-path-mapping \
    --domain-name api.example.com \
    --rest-api-id <rest-api-id> \
    --stage prod \
    --base-path orders
```

## To list the mappings of a domain

The following example returns every stage mapped onto the domain.

```bash
aws apigateway get-base-path-mappings --domain-name api.example.com
```

## To create a VPC link

The following example creates a V2 link. Put the subnets in at least two
availability zones, otherwise the link fails health checks.

```bash
aws apigateway create-vpc-link \
    --name orders-link \
    --vpc-link-version V2 \
    --subnet-ids subnet-<id-1> subnet-<id-2> \
    --security-group-ids sg-<id>
```

## To list the VPC links

The following example returns the links of a region. The call is paginated, so
repeat it with `--position`.

```bash
aws apigateway get-vpc-links --max-results 500
```

## To send an integration through a VPC link

The following example proxies to an internal load balancer over the link from
[Integrations](integrations.md).

```bash
aws apigateway put-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --type HTTP_PROXY \
    --integration-http-method GET \
    --uri http://internal.example.com/orders \
    --connection-type VPC_LINK \
    --vpc-link-id <vpc-link-id>
```

## To delete a base path mapping

The following example removes one mapping and leaves the domain in place.

```bash
aws apigateway delete-base-path-mapping \
    --domain-name api.example.com \
    --base-path orders
```

## To delete a domain name

The following example removes a domain. Every mapping on it has to go first,
otherwise the call fails.

```bash
aws apigateway delete-domain-name --domain-name api.example.com
```