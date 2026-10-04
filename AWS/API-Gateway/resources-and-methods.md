# Resources and Methods

> Resources and Methods. Part of the [API-Gateway](../) cheatsheet.

A resource is one path part, and the root of the tree is the resource with the
path `/`. A method is a verb on a resource. Wires are attached afterwards, in
[Integrations](integrations.md).

## To list the resource tree

The following example returns every path with its methods attached. The call is
paginated, so repeat it with `--position` until the response has no `position`
field.

```bash
aws apigateway get-resources \
    --rest-api-id <rest-api-id> \
    --limit 500 \
    --embed methods
```

## To find the root resource id

The following example returns the id of the resource whose path is `/`, which
every other path is created under.

```bash
aws apigateway get-resources \
    --rest-api-id <rest-api-id> \
    --query 'items[?path==`/`].id | [0]'
```

## To create a path part

The following example adds `/orders` under the root resource.

```bash
aws apigateway create-resource \
    --rest-api-id <rest-api-id> \
    --parent-id <root-resource-id> \
    --path-part orders
```

## To create a nested path part

The following example adds `/orders/{orderId}` under the resource created
above. A path part is one segment, so the braces are part of the name.

```bash
aws apigateway create-resource \
    --rest-api-id <rest-api-id> \
    --parent-id <orders-resource-id> \
    --path-part '{orderId}'
```

## To add a method with no authorization

The following example adds an open `GET`. Nothing can be called until an
integration exists, so a new method returns `500` until then.

```bash
aws apigateway put-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --authorization-type NONE \
    --api-key-required false
```

## To require an API key and a query string parameter

The following example adds a `POST` that needs a key and accepts an optional
`dryRun` query string parameter. Declaring a parameter is what makes it visible
to the method response and the integration.

```bash
aws apigateway put-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method POST \
    --authorization-type NONE \
    --api-key-required true \
    --request-parameters 'method.request.querystring.dryRun=true'
```

## To protect a method with an authorizer

The following example adds a method that calls the authorizer in
[Authorizers](authorizers.md) before the integration runs.

```bash
aws apigateway put-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method POST \
    --authorization-type CUSTOM \
    --authorizer-id <authorizer-id>
```

## To change the authorization of an existing method

The following example replaces the authorizer with an IAM check, which
requires the caller to have `execute-api:Invoke` on the method.

```bash
aws apigateway update-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --patch-operations op=replace,path=/authorizationType,value=AWS_IAM
```

## To read a method

The following example returns one method with its request parameters and
integration summary.

```bash
aws apigateway get-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET
```

## To test a method without deploying it

The following example calls the method straight against the API Gateway test
harness. `response` is what a client would receive and `body` is the raw backend
output, which is how you compare the two.

```bash
aws apigateway test-invoke-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method POST \
    --path '/orders?dryRun=true' \
    --body '{"id":1}'
```

## To delete a method

The following example removes a method and its integration. There is no dry
run.

```bash
aws apigateway delete-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method DELETE
```

## To delete a path part

The following example removes a resource and everything under it. Check
`get-resources` first, because a path part cannot be removed while a child still
references it. There is no dry run.

```bash
aws apigateway delete-resource \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id>
```