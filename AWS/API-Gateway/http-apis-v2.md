# HTTP APIs (v2)

> HTTP APIs (v2). Part of the [API-Gateway](../) cheatsheet.

An HTTP API is cheaper and faster than a REST API and has no deployments,
method responses, or integration responses: the `$default` stage serves changes
as soon as they are saved. The CLI is `aws apigwv2` and the ids are `api-id`
rather than `rest-api-id`.

## To create an HTTP API in front of a Lambda function

The following example creates the API with the function as its target, which
wires a proxy integration in one step.

```bash
aws apigwv2 create-api \
    --name orders-http-api \
    --protocol-type HTTP \
    --target <function-arn>
```

## To list the HTTP APIs of a region

The following example returns the id and name of every HTTP API. The call is
paginated, so repeat it with `--next-token`.

```bash
aws apigwv2 get-apis --max-results 500
```

## To add a second backend to an API

The following example adds an AWS proxy integration to the function.

```bash
aws apigwv2 create-integration \
    --api-id <api-id> \
    --integration-type AWS_PROXY \
    --integration-uri <function-arn> \
    --payload-format-version 2.0
```

## To route a path to the integration

The following example sends `GET /orders` to the integration created above.

```bash
aws apigwv2 create-route \
    --api-id <api-id> \
    --route-key 'GET /orders' \
    --target integrations/<integration-id>
```

## To send every request to one integration

The following example points the `$default` route at an integration, which is
what a single-purpose API needs.

```bash
aws apigwv2 update-route \
    --api-id <api-id> \
    --route-id '$default' \
    --target integrations/<integration-id>
```

## To list the routes of an API

The following example returns the route keys and their targets.

```bash
aws apigwv2 get-routes --api-id <api-id>
```

## To create the $default stage

The following example creates the stage that serves the invoke URL. Without
`--auto-deploy`, later changes need their own stage.

```bash
aws apigwv2 create-stage \
    --api-id <api-id> \
    --stage-name '$default' \
    --auto-deploy
```

## To add a JWT authorizer

The following example validates the signature and the audience of a token from
an identity provider, which is the usual way to secure an HTTP API.

```bash
aws apigwv2 create-authorizer \
    --api-id <api-id> \
    --name jwt-auth \
    --type JWT \
    --identity-sources 'method.request.header.Authorization' \
    --jwt-configuration Audience=<audience>,Issuer=<issuer>
```

## To require the authorizer on a route

The following example protects one route. Without this, adding the authorizer
changes nothing.

```bash
aws apigwv2 update-route \
    --api-id <api-id> \
    --route-id <route-id> \
    --authorization-type JWT \
    --authorizer-id <authorizer-id>
```

## To add a custom domain

The following example creates a domain for the API with an ACM certificate.

```bash
aws apigwv2 create-domain-name \
    --domain-name api.example.com \
    --domain-name-configuration DomainNameType=custom,SecurityPolicy=TLS_1_2,CertificateArn=<certificate-arn>
```

## To map the API onto the domain

The following example serves the API from the root of the domain. Use a key such
as `orders` to share the domain with another API.

```bash
aws apigwv2 create-api-mapping \
    --api-id <api-id> \
    --domain-name api.example.com \
    --stage '$default' \
    --api-mapping-key '(none)'
```

## To delete an HTTP API

The following example deletes an API, its routes, and its stages. There is no
dry run, and an import cannot be merged into an existing API the way it can for
REST.

```bash
aws apigwv2 delete-api --api-id <api-id>
```