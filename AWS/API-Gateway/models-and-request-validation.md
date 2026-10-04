# Models and Request Validation

> Models and Request Validation. Part of the [API-Gateway](../) cheatsheet.

A model is a JSON Schema, and a request validator decides whether a method
checks the body against it. Both are attached in
[Resources and Methods](resources-and-methods.md).

## To define a request model

The following example defines the shape of an order. The schema is a string, so
a large definition is easier to pass from a file.

```bash
aws apigateway create-model \
    --rest-api-id <rest-api-id> \
    --name Order \
    --content-type application/json \
    --schema '{"type":"object","required":["id"],"properties":{"id":{"type":"string"},"total":{"type":"number"}}}'
```

## To list the models of an API

The following example returns every model defined on the API.

```bash
aws apigateway get-models --rest-api-id <rest-api-id>
```

## To create a request validator

The following example checks both the body and the query string parameters of a
method.

```bash
aws apigateway create-request-validator \
    --rest-api-id <rest-api-id> \
    --name ValidateOrder \
    --validate-request-body true \
    --validate-request-parameters true
```

## To attach the model to a method

The following example maps the `application/json` body of a `POST` to the model.
The model is referenced by ARN, not by name.

```bash
aws apigateway put-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method POST \
    --authorization-type NONE \
    --request-models '{"application/json":"arn:aws:apigateway:us-east-1::restapis/<rest-api-id>:models:Order"}'
```

## To turn validation on for the method

The following example attaches the validator to the method. A request that fails
validation gets a `400` before the integration runs.

```bash
aws apigateway update-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method POST \
    --patch-operations op=add,path=/requestValidatorId,value=<request-validator-id>
```

## To list the validators of an API

The following example returns the validators and shows which body validator is
`NONE`.

```bash
aws apigateway get-request-validators --rest-api-id <rest-api-id>
```

## To delete a validator

The following example removes a validator. A method that still points at it
starts rejecting every request, so detach it first.

```bash
aws apigateway delete-request-validator \
    --rest-api-id <rest-api-id> \
    --request-validator-id <request-validator-id>
```

## To delete a model

The following example removes a model. A method that still references it stops
validating its body.

```bash
aws apigateway delete-model \
    --rest-api-id <rest-api-id> \
    --model-name Order
```