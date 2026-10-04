# Method and Gateway Responses

> Method and Gateway Responses. Part of the [API-Gateway](../) cheatsheet.

A method response declares what a method can return to a client, and an
integration response in [Integrations](integrations.md) maps a backend reply
onto it. A gateway response covers the replies API Gateway itself generates,
such as a missing API key.

## To declare the response a method returns

The following example declares a `200` with one response header. A header has
to be declared here before it can be mapped from the integration.

```bash
aws apigateway put-method-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "200" \
    --response-parameters 'method.response.header.Content-Type=true'
```

## To declare the CORS headers on a method response

The following example declares three headers in one call. The order of the
pairs does not matter.

```bash
aws apigateway put-method-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "200" \
    --response-parameters 'method.response.header.Access-Control-Allow-Origin=true,method.response.header.Access-Control-Allow-Methods=true,method.response.header.Access-Control-Allow-Headers=true'
```

## To declare a client error response

The following example adds a `400` to a method, so an integration can map a
backend error onto it.

```bash
aws apigateway put-method-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "400"
```

## To list the responses of a method

The following example returns every declared method response.

```bash
aws apigateway get-method-responses \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET
```

## To remove a method response

The following example removes one declared response. A method needs at least
one response left.

```bash
aws apigateway delete-method-response \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --status-code "400"
```

## To rewrite the body of an API Gateway error

The following example replaces the default `missing authentication token`
payload with the message API Gateway already holds, which is the quickest way to
return a consistent error shape.

```bash
aws apigateway put-gateway-response \
    --rest-api-id <rest-api-id> \
    --response-type DEFAULT_4XX \
    --status-code "400" \
    --response-parameters 'gatewayresponse.header.Authorization=method.request.header.Authorization' \
    --response-templates 'application/json={"message":$context.error.messageString}'
```

## To return an error body for an unknown route

The following example rewrites the body for a request to a path that does not
exist, which is the `MISSING_AUTHENTICATION_TOKEN` and `MISSING_ROUTE` family of
errors.

```bash
aws apigateway put-gateway-response \
    --rest-api-id <rest-api-id> \
    --response-type MISSING_AUTHENTICATION_TOKEN
```

## To list the gateway responses

The following example returns the four response types an API has:
`DEFAULT_4XX`, `DEFAULT_5XX`, `ACCESS_DENIED`, and
`RESOURCE_NOT_FOUND`.

```bash
aws apigateway get-gateway-responses --rest-api-id <rest-api-id>
```

## To update an existing gateway response

The following example adds a header to the `DEFAULT_5XX` response, which is the
way to make a backend failure visible in the browser instead of an opaque
`Internal server error`.

```bash
aws apigateway update-gateway-response \
    --rest-api-id <rest-api-id> \
    --response-type DEFAULT_5XX \
    --response-parameters 'gatewayresponse.header.Access-Control-Allow-Origin=method.request.header.Origin'
```