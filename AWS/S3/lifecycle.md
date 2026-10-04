# Lifecycle

> Lifecycle. Part of the [S3](../) cheatsheet.

## To view the current lifecycle rules

The following example returns the bucket's lifecycle configuration.

```bash
aws s3api get-bucket-lifecycle-configuration --bucket <bucket>
```

## To apply a lifecycle policy

The following example applies a lifecycle document that transitions objects to
Standard-IA after 30 days and expires them after a year.

```bash
aws s3api put-bucket-lifecycle-configuration \
    --bucket <bucket> \
    --lifecycle-configuration file://lifecycle.json
```

`lifecycle.json`:

```json
{
  "Rules": [
    {
      "ID": "MoveToIA",
      "Status": "Enabled",
      "Filter": { "Prefix": "" },
      "Transitions": [
        { "Days": 30, "StorageClass": "STANDARD_IA" }
      ],
      "Expiration": { "Days": 365 }
    }
  ]
}
```
