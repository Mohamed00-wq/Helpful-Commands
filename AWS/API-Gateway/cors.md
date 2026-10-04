# CORS

> CORS. Part of the [API-Gateway](../) cheatsheet.

CORS is decided by API Gateway, not by the backend, so every path that a browser
can reach needs an `OPTIONS` method that returns the headers. The four commands
below are the whole setup.

## To add an OPTIONS method for preflight

The following example adds an unauthenticated `OPTIONS` to a resource. The
method must exist before the headers can be attached to it.

```bash
aws apigateway put-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method OPTIONS \
    --authorization-type NONE \
    --api-key-required false
```

## To answer the preflight from API Gateway

The following example attaches a mock integration, so the preflight never
reaches the backend and costs nothing.

```bash
aws apigateway put-integration \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method OPTIONS \
    --type MOCK \
    --request-templates '{"application/json": "{\"statusCode\": 200}"}'
```

## To add the CORS headers to the preflight response

The following example declares the three headers a browser reads. The actual
values come from the integration response in the next step.

```bash
aws apigateway put-method-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method OPTIONS \
    --status-code "200" \
    --response-parameters 'method.response.header.Access-Control-Allow-Headers=true,method.response.header.Access-Control-Allow-Methods=true,method.response.header.Access-Control-Allow-Origin=true'
```

## To carry the headers through the mock integration

The following example copies each header from the integration response to the
method response, which is what puts it on the wire.

```bash
aws apigateway put-integration-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method OPTIONS \
    --status-code "200" \
    --response-parameters 'method.response.header.Access-Control-Allow-Headers=integration.response.header.Access-Control-Allow-Headers,method.response.header.Access-Control-Allow-Methods=integration.response.header.Access-Control-Allow-Methods,method.response.header.Access-Control-Allow-Origin=integration.response.header.Access-Control-Allow-Origin'
```

## To allow several origins

The following example echoes the caller's `Origin` instead of naming one, so the
same API works from several domains.

```bash
aws apigateway put-integration-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method OPTIONS \
    --status-code "200" \
    --response-parameters 'method.response.header.Access-Control-Allow-Origin=method.request.header.Origin'
```

## To add the headers to API Gateway's own errors

The following example adds the headers to `DEFAULT_4XX`, so a rejected request
is readable by the browser rather than failing as an opaque network error. The
same call is worth repeating for `DEFAULT_5XX`.

```bash
aws apigateway put-gateway-response \
    --rest-api-id <rest-api-id> \
    --response-type DEFAULT_4XX \
    --response-parameters 'gatewayresponse.header.Access-Control-Allow-Origin=method.request.header.Origin,gatewayresponse.header.Access-Control-Allow-Headers=Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Amz-Security-Token,method.request.header.Origin'
```

## To add the headers to the real methods

The following example declares the headers on the method that answers the
browser, in [Method and Gateway Responses](method-and-gateway-responses.md).

```bash
aws apigateway put-method-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "200" \
    --response-parameters 'method.response.header.Access-Control-Allow-Origin=true'
```