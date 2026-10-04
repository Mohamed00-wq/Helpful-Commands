# Cache Policies

> Cache Policies. Part of the [CloudFront](../) cheatsheet.

## To list all cache policies

The following example lists all cache policies.

```bash
aws cloudfront list-cache-policies
```

## To get cache policy details

The following example gets cache policy details.

```bash
aws cloudfront get-cache-policy --id E1234567890ABC
```

## To create a cache policy

The following example creates a cache policy.

```bash
aws cloudfront create-cache-policy
    --cache-policy-config '{"Name":"my-policy","DefaultTtlConfig":{"DefaultTtl":86400},"MaxTtlConfig":{"MaxTtl":31536000},"MinTtlConfig":{"MinTtl":0},"ParametersInCacheKeyAndForwardedToOrigin":{"EnableAcceptEncodingGzip":true,"EnableAcceptEncodingBrotli":true,"HeadersConfig":{"HeaderBehavior":"none"},"CookiesConfig":{"CookieBehavior":"none"},"QueryStringsConfig":{"QueryStringBehavior":"all"}}}'
```

## To delete a cache policy

The following example deletes a cache policy.

```bash
aws cloudfront delete-cache-policy --id E1234567890ABC
```
