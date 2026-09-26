# CloudFront (Content Delivery Network)

> Commands for distributions, invalidations, origin access control, and cache policies.

## Distributions

### To list all distributions

### To get distribution details

The following example gets distribution details.

```bash
aws cloudfront get-distribution --id E1234567890ABC
```

### To create a distribution

The following example creates a distribution.

```bash
aws cloudfront create-distribution --distribution-config file://dist-config.json
```

### To update a distribution

The following example updates a distribution.

```bash
aws cloudfront update-distribution
    --id E1234567890ABC
    --distribution-config file://dist-config.json
    --if-match E1234567890ABC
```

### To delete a distribution

The following example deletes a distribution.

```bash
aws cloudfront delete-distribution --id E1234567890ABC --if-match E1234567890ABC
```

### To disable a distribution

The following example disables a distribution.

```bash
aws cloudfront disable-distribution --id E1234567890ABC --if-match E1234567890ABC
```

### To enable a distribution

The following example enables a distribution.

```bash
aws cloudfront enable-distribution --id E1234567890ABC --if-match E1234567890ABC
```

## Invalidation

### To invalidate all files

The following example invalidates all files.

```bash
aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/*"
```

### To invalidate specific paths

The following example invalidates specific paths.

```bash
aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/images/*" "/css/*"
```

### To list invalidations

The following example lists invalidations.

```bash
aws cloudfront list-invalidations --distribution-id E1234567890ABC
```

### To check invalidation status

The following example checks invalidation status.

```bash
aws cloudfront get-invalidation --distribution-id E1234567890ABC --id I1234567890ABC
```

## Origin Access Control (OAC)

### To list OACs

The following example lists OACs.

```bash
aws cloudfront list-origin-access-controls
```

### To create an OAC

The following example creates an OAC.

```bash
aws cloudfront create-origin-access-control
    --origin-access-control-config '{"Name":"my-oac","SigningBehavior":"always","SigningProtocol":"sigv4","OriginAccessControlOriginType":"s3"}'
```

### To delete an OAC

The following example deletes an OAC.

```bash
aws cloudfront delete-origin-access-control --id E1234567890ABC
```

## S3 Bucket Policy for CloudFront

### To set CloudFront bucket policy

The following example sets CloudFront bucket policy.

```bash
aws s3api put-bucket-policy --bucket my-bucket --policy file://cf-policy.json
```

## Cache Policies

### To list all cache policies

The following example lists all cache policies.

```bash
aws cloudfront list-cache-policies
```

### To get cache policy details

The following example gets cache policy details.

```bash
aws cloudfront get-cache-policy --id E1234567890ABC
```

### To create a cache policy

The following example creates a cache policy.

```bash
aws cloudfront create-cache-policy
    --cache-policy-config '{"Name":"my-policy","DefaultTtlConfig":{"DefaultTtl":86400},"MaxTtlConfig":{"MaxTtl":31536000},"MinTtlConfig":{"MinTtl":0},"ParametersInCacheKeyAndForwardedToOrigin":{"EnableAcceptEncodingGzip":true,"EnableAcceptEncodingBrotli":true,"HeadersConfig":{"HeaderBehavior":"none"},"CookiesConfig":{"CookieBehavior":"none"},"QueryStringsConfig":{"QueryStringBehavior":"all"}}}'
```

### To delete a cache policy

The following example deletes a cache policy.

```bash
aws cloudfront delete-cache-policy --id E1234567890ABC
```

## Real-Time Logs

### To list distribution IDs

The following example lists distribution IDs.

```bash
aws cloudfront list-distributions --query "DistributionList.Items[*].{Id:Id,DomainName:DomainName}"
```

```bash
# Real-time logs require Kinesis Data Stream + CloudWatch Logs
# See AWS docs for full setup
```
