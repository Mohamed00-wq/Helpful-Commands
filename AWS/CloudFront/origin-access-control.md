# Origin Access Control (OAC)

> Origin Access Control (OAC). Part of the [CloudFront](../CloudFront.md) cheatsheet.

## To list OACs

The following example lists OACs.

```bash
aws cloudfront list-origin-access-controls
```

## To create an OAC

The following example creates an OAC.

```bash
aws cloudfront create-origin-access-control
    --origin-access-control-config '{"Name":"my-oac","SigningBehavior":"always","SigningProtocol":"sigv4","OriginAccessControlOriginType":"s3"}'
```

## To delete an OAC

The following example deletes an OAC.

```bash
aws cloudfront delete-origin-access-control --id E1234567890ABC
```
