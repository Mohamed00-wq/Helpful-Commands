# Integrations

> Integrations. Part of the [API-Gateway](../) cheatsheet.

An integration is the backend behind a method in
[Resources and Methods](resources-and-methods.md). The integration type decides
how much mapping work there is: `AWS_PROXY` and `HTTP_PROXY` hand the request to
the backend unchanged, and the other types use the responses in
[Method and Gateway Responses](method-and-gateway-responses.md).

## To connect a method to a Lambda function with proxy integration

The following example wires a method straight to a function. A proxy
integration needs no method response and no integration response, because the
function returns the status code, headers, and body itself.

```bash
aws apigateway put-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --type AWS_PROXY \
    --integration-http-method POST \
    --uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/<function-arn>/invocations
```

## To proxy to another AWS service

The following example proxies to S3 and maps two query string parameters onto
the path of the bucket and key.

```bash
aws apigateway put-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --type AWS \
    --integration-http-method POST \
    --uri 'arn:aws:apigateway:us-east-1:s3:path/{Bucket}/{Key+}' \
    --credentials arn:aws:iam::<account-id>:role/api-gateway-s3-read-role \
    --request-parameters '{"integration.request.path.Bucket":"method.request.querystring.bucket","integration.request.path.Key":"method.request.querystring.key"}'
```

## To use a non-proxy Lambda integration

The following example maps requests and responses by hand, which is what
`put-integration-response` in
[Method and Gateway Responses](method-and-gateway-responses.md) completes. A
non-proxy integration always needs `--credentials`, an IAM role that API Gateway
assumes to call the backend.

```bash
aws apigateway put-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method POST \
    --type AWS \
    --integration-http-method POST \
    --uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/<function-arn>/invocations \
    --credentials arn:aws:iam::<account-id>:role/api-gateway-invoke-role \
    --passthrough-behavior WHEN_NO_MATCH \
    --connection-type INTERNET \
    --timeout-in-millis 29000
```

## To map the backend status code to a client status code

The following example maps a backend `2xx` to a client `200`. A response the
selection pattern does not match turns into a `5XX` unless another integration
response catches it.

```bash
aws apigateway put-integration-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "200" \
    --selection-pattern '2\d{2}' \
    --response-parameters 'method.response.header.Content-Type=integration.response.header.Content-Type'
```

## To catch backend client errors

The following example maps a backend `4xx` to a client `400`.

```bash
aws apigateway put-integration-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "400" \
    --selection-pattern '4\d{2}'
```

## To inspect an integration

The following example returns the integration behind one method.

```bash
aws apigateway get-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET
```

## To list the integrations of a resource

The following example returns one integration per method on the resource.

```bash
aws apigateway get-integrations \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id>
```

## To delete an integration

The following example removes one integration and leaves the method in place,
which then returns `500`.

```bash
aws apigateway delete-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET
```

## To remove every integration on a resource

The following example deletes all integrations under a resource in one call,
which is the quickest way to start a resource over.

```bash
aws apigateway delete-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --delete-all-integrations
```