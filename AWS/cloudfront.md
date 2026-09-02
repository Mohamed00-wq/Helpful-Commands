# 🚀 CloudFront (CDN)

> Advanced CloudFront CLI commands for distributions, invalidations, and origin access — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Distributions

| Command | Description |
|---|---|
| `aws cloudfront list-distributions` | List all distributions |
| `aws cloudfront get-distribution --id E1234567890ABC` | Get distribution details |
| `aws cloudfront create-distribution --distribution-config file://dist-config.json` | Create a distribution |
| `aws cloudfront update-distribution --id E1234567890ABC --distribution-config file://dist-config.json --if-match E1234567890ABC` | Update a distribution |
| `aws cloudfront delete-distribution --id E1234567890ABC --if-match E1234567890ABC` | Delete a distribution |
| `aws cloudfront disable-distribution --id E1234567890ABC --if-match E1234567890ABC` | Disable a distribution |
| `aws cloudfront enable-distribution --id E1234567890ABC --if-match E1234567890ABC` | Enable a distribution |

```bash
# Create distribution
aws cloudfront create-distribution \
  --distribution-config file://dist-config.json

# Disable before deletion
aws cloudfront disable-distribution \
  --id E1234567890ABC \
  --if-match E1234567890ABC

aws cloudfront delete-distribution \
  --id E1234567890ABC \
  --if-match E1234567890ABC
```

---

## Invalidation

| Command | Description |
|---|---|
| `aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/*"` | Invalidate all files |
| `aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/images/*" "/css/*"` | Invalidate specific paths |
| `aws cloudfront list-invalidations --distribution-id E1234567890ABC` | List invalidations |
| `aws cloudfront get-invalidation --distribution-id E1234567890ABC --id I1234567890ABC` | Check invalidation status |

```bash
# Invalidate all cached content
aws cloudfront create-invalidation \
  --distribution-id E1234567890ABC \
  --paths "/*"

# Invalidate specific paths
aws cloudfront create-invalidation \
  --distribution-id E1234567890ABC \
  --paths "/images/*" "/css/*"
```

---

## Origin Access Control (OAC)

| Command | Description |
|---|---|
| `aws cloudfront list-origin-access-controls` | List OACs |
| `aws cloudfront create-origin-access-control --origin-access-control-config '{"Name":"my-oac","SigningBehavior":"always","SigningProtocol":"sigv4","OriginAccessControlOriginType":"s3"}'` | Create an OAC |
| `aws cloudfront delete-origin-access-control --id E1234567890ABC` | Delete an OAC |

```bash
aws cloudfront create-origin-access-control \
  --origin-access-control-config '{"Name":"my-oac","SigningBehavior":"always","SigningProtocol":"sigv4","OriginAccessControlOriginType":"s3"}'
```

---

## S3 Bucket Policy for CloudFront

| Command | Description |
|---|---|
| `aws s3api put-bucket-policy --bucket my-bucket --policy file://cf-policy.json` | Set CloudFront bucket policy |

```bash
# cf-policy.json:
# {
#   "Version": "2012-10-17",
#   "Statement": [{
#     "Sid": "AllowCloudFrontServicePrincipal",
#     "Effect": "Allow",
#     "Principal": {"Service": "cloudfront.amazonaws.com"},
#     "Action": "s3:GetObject",
#     "Resource": "arn:aws:s3:::my-bucket/*",
#     "Condition": {
#       "StringEquals": {
#         "AWS:SourceArn": "arn:aws:cloudfront::ACCOUNT:distribution/E1234567890ABC"
#       }
#     }
#   }]
# }

aws s3api put-bucket-policy \
  --bucket my-bucket \
  --policy file://cf-policy.json
```

---

## Cache Policies

| Command | Description |
|---|---|
| `aws cloudfront list-cache-policies` | List all cache policies |
| `aws cloudfront get-cache-policy --id E1234567890ABC` | Get cache policy details |
| `aws cloudfront create-cache-policy --cache-policy-config '{"Name":"my-policy","DefaultTtlConfig":{"DefaultTtl":86400},"MaxTtlConfig":{"MaxTtl":31536000},"MinTtlConfig":{"MinTtl":0},"ParametersInCacheKeyAndForwardedToOrigin":{"EnableAcceptEncodingGzip":true,"EnableAcceptEncodingBrotli":true,"HeadersConfig":{"HeaderBehavior":"none"},"CookiesConfig":{"CookieBehavior":"none"},"QueryStringsConfig":{"QueryStringBehavior":"all"}}}'` | Create a cache policy |
| `aws cloudfront delete-cache-policy --id E1234567890ABC` | Delete a cache policy |

```bash
aws cloudfront create-cache-policy \
  --cache-policy-config '{"Name":"my-policy","DefaultTtlConfig":{"DefaultTtl":86400},"MaxTtlConfig":{"MaxTtl":31536000},"MinTtlConfig":{"MinTtl":0},"ParametersInCacheKeyAndForwardedToOrigin":{"EnableAcceptEncodingGzip":true,"EnableAcceptEncodingBrotli":true,"HeadersConfig":{"HeaderBehavior":"none"},"CookiesConfig":{"CookieBehavior":"none"},"QueryStringsConfig":{"QueryStringBehavior":"all"}}}'
```

---

## Real-Time Logs

| Command | Description |
|---|---|
| `aws cloudfront list-distributions --query "DistributionList.Items[*].{Id:Id,DomainName:DomainName}"` | List distribution IDs |

```bash
# Real-time logs require Kinesis Data Stream + CloudWatch Logs
# See AWS docs for full setup
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
