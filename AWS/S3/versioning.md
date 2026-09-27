# Versioning

> Versioning. Part of the [S3](../S3.md) cheatsheet.

## To check the versioning status

The following example reports whether versioning is enabled, suspended, or
never configured.

```bash
aws s3api get-bucket-versioning --bucket <bucket>
```

## To enable versioning

The following example enables versioning. It cannot be turned off afterwards,
only suspended.

```bash
aws s3api put-bucket-versioning \
    --bucket <bucket> \
    --versioning-configuration Status=Enabled
```

## To list object versions

The following example lists every version of the objects under a prefix.

```bash
aws s3api list-object-versions --bucket <bucket> --prefix <prefix>
```

## To delete a specific object version

The following example removes one version, which permanently destroys that
copy of the object.

```bash
aws s3api delete-object --bucket <bucket> --key <key> --version-id <version-id>
```
