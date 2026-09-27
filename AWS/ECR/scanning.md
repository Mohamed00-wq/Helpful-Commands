# Scanning

> Scanning. Part of the [ECR](../ECR.md) cheatsheet.

## To start an on-demand scan

The following example scans an image even when scan on push is disabled.

```bash
aws ecr start-image-scan \
    --repository-name <repository> \
    --image-id imageTag=<tag>
```

## To check the scan findings

The following example lists findings. `HIGH` and `CRITICAL` are the severities
worth failing a build on.

```bash
aws ecr describe-image-scan-findings \
    --repository-name <repository> \
    --image-id imageTag=<tag> \
    --query 'imageScanFindings.findings[?severity==`HIGH` || severity==`CRITICAL`].[name,severity]' \
    --output table
```
