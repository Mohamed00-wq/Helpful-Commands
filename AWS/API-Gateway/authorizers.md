# Authorizers

> Authorizers. Part of the [API-Gateway](../) cheatsheet.

An authorizer decides whether a request may reach the backend, and it runs
before the integration. It is attached to a method in
[Resources and Methods](resources-and-methods.md).

## To create a Cognito authorizer

The following example accepts a JWT from a Cognito user pool. The pool is named
by ARN and the identity source is the header that carries the token.

```bash
aws apigateway create-authorizer \
    --rest-api-id <rest-api-id> \
    --name CognitoAuth \
    --type COGNITO_USER_POOLS \
    --provider-arns arn:aws:cognito-idp:us-east-1:<account-id>:userpool/us-east-1_example \
    --identity-source method.request.header.Authorization
```

## To create a Lambda authorizer

The following example calls a function that returns an IAM policy. The function
needs permission to be invoked by API Gateway, and the role in
`--authorizer-credentials` is what it receives.

```bash
aws apigateway create-authorizer \
    --rest-api-id <rest-api-id> \
    --name LambdaAuth \
    --type REQUEST \
    --authorizer-uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/<function-arn>/invocations \
    --authorizer-credentials arn:aws:iam::<account-id>:role/api-gateway-authorizer-role \
    --identity-source method.request.header.Authorization \
    --identity-validation-expression 'Bearer .+' \
    --authorizer-result-ttl-in-seconds 300
```

## To read an identity from a query string parameter

The following example makes the identity source a header or a parameter, which
suits internal callers that cannot set headers.

```bash
aws apigateway create-authorizer \
    --rest-api-id <rest-api-id> \
    --name HeaderOrQueryAuth \
    --type REQUEST \
    --authorizer-uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/<function-arn>/invocations \
    --authorizer-credentials arn:aws:iam::<account-id>:role/api-gateway-authorizer-role \
    --identity-source method.request.header.Authorization,method.request.querystring.token
```

## To list the authorizers of an API

The following example returns every authorizer with its type and TTL.

```bash
aws apigateway get-authorizers --rest-api-id <rest-api-id>
```

## To change how long a result is cached

The following example caches the authorizer result for one minute instead of
five, which is the setting to lower when access is revoked quickly.

```bash
aws apigateway update-authorizer \
    --rest-api-id <rest-api-id> \
    --authorizer-id <authorizer-id> \
    --patch-operations op=replace,path=/authorizerResultTtlInSeconds,value=60
```

## To test an authorizer

The following example calls the authorizer without going through a method, so
the policy it returns can be read on its own.

```bash
aws apigateway test-invoke-authorizer \
    --rest-api-id <rest-api-id> \
    --authorizer-id <authorizer-id> \
    --identity-source "Bearer <token>"
```

## To delete an authorizer

The following example removes an authorizer. Every method still pointing at it
starts failing, so check with `get-authorizers` first.

```bash
aws apigateway delete-authorizer \
    --rest-api-id <rest-api-id> \
    --authorizer-id <authorizer-id>
```