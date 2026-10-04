# OpenAPI Import and Export

> OpenAPI Import and Export. Part of the [API-Gateway](../) cheatsheet.

An export is the fastest way to see what an API actually contains, because it
returns the definition rather than the API Gateway view of it. See
[APIs](apis.md) for the API itself.

## To export an API as OpenAPI 3.0

The following example writes the definition to `apigateway_export.json`. Pass
`--stage-name` to export a specific deployment instead of the most recent one.

```bash
aws apigateway get-export \
    --rest-api-id <rest-api-id> \
    --export-type oas30 \
    --stage-name prod
```

## To export an API as Swagger 2.0

The following example writes a Swagger 2.0 document to
`apigateway_export.json`, which is the format to edit by hand before an import.

```bash
aws apigateway get-export \
    --rest-api-id <rest-api-id> \
    --export-type swagger
```

## To export an API without API Gateway extensions

The following example drops the `x-amazon-apigateway-*` extensions, leaving a
document that other tools can read.

```bash
aws apigateway get-export \
    --rest-api-id <rest-api-id> \
    --export-type oas30 \
    --no-dependencies
```

## To import a definition into a new API

The following example creates an API from a local OpenAPI document. `--fail-on-errors`
returns a `400` with the offending path instead of logging the problem and
carrying on.

```bash
aws apigateway import-rest-api \
    --body file://openapi.json \
    --parameters endpointConfigurationTypes=REGIONAL \
    --fail-on-errors
```

## To merge a definition into an existing API

The following example updates the paths of `put` and `delete`, because
`overwrite` merges the incoming definition over what is already deployed.

```bash
aws apigateway import-rest-api \
    --body file://openapi.json \
    --rest-api-id <rest-api-id> \
    --parameters mergeBehavior=overwrite,endpointConfigurationTypes=REGIONAL \
    --fail-on-errors
```

## To copy an API

The following example stores the current definition, which the import example
above can then load into another API.

```bash
aws apigateway get-rest-api \
    --rest-api-id <rest-api-id> \
    --embed methods,resources \
    --output json > api.json
```

## To export an HTTP API

The following example writes an HTTP API definition to `apigwv2_export.json`.
An HTTP API cannot be imported over an existing one, so importing means
creating a new API and deleting the old one.

```bash
aws apigwv2 get-export \
    --api-id <api-id> \
    --output-type JSON \
    --specification OAS30
```