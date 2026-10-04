# Layers

> Layers. Part of the [Lambda](../) cheatsheet.

## To list all layers

The following example lists all layers.

```bash
aws lambda list-layers
```

## To publish a layer version

The following example publishes a layer version.

```bash
aws lambda publish-layer-version
    --layer-name my-layer
    --zip-file fileb://layer.zip
    --compatible-runtimes python3.12
```

## To get layer version details

The following example gets layer version details.

```bash
aws lambda get-layer-version --layer-name my-layer --version-number 1
```

## To list layer versions

The following example lists layer versions.

```bash
aws lambda list-layer-versions --layer-name my-layer
```

## To delete a layer version

The following example deletes a layer version.

```bash
aws lambda delete-layer-version --layer-name my-layer --version-number 1
```

## To attach a layer

The following example attaches a layer.

```bash
aws lambda update-function-configuration
    --function-name my-function
    --layers arn:aws:lambda:us-east-1:ACCOUNT:layer:my-layer:1
```
