# Static Website Hosting

> Static Website Hosting. Part of the [S3](../) cheatsheet.

## To configure a website endpoint

The following example sets the index and error documents for static website
hosting.

```bash
aws s3 website s3://<bucket> \
    --index-document index.html \
    --error-document error.html
```

The website is then served from
`http://<bucket>.s3-website-<region>.amazonaws.com`. This endpoint only supports
public buckets, and it cannot be served over HTTPS. For a private bucket, put
CloudFront in front of it with Origin Access Control instead.
