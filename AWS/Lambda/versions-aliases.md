# Versions & Aliases

> Versions & Aliases. Part of the [Lambda](../) cheatsheet.

## To list all versions

The following example lists all versions.

```bash
aws lambda list-versions-by-function --function-name my-function
```

## To publish a new version

The following example publishes a new version.

```bash
aws lambda publish-version --function-name my-function
```

## To publish with description

The following example publishes with description.

```bash
aws lambda publish-version --function-name my-function --description "v2 release"
```

## To list aliases

The following example lists aliases.

```bash
aws lambda list-aliases --function-name my-function
```

## To create an alias

The following example creates an alias.

```bash
aws lambda create-alias --function-name my-function --name PROD --function-version 2
```

## To update an alias

The following example updates an alias.

```bash
aws lambda update-alias --function-name my-function --name PROD --function-version 3
```

## To delete an alias

The following example deletes an alias.

```bash
aws lambda delete-alias --function-name my-function --name PROD
```

## To get specific version/alias

The following example gets specific version/alias.

```bash
aws lambda get-function --function-name my-function --qualifier PROD
```

```bash
# Publish version
aws lambda publish-version --function-name my-function

# Create alias
aws lambda create-alias \
    --function-name my-function \
    --name PROD \
    --function-version 2

# Invoke specific version
aws lambda invoke \
    --function-name my-function:PROD \
    --payload '{}' output.json
```
