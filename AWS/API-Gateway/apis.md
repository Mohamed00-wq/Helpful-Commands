# APIs

> APIs. Part of the [API-Gateway](../) cheatsheet.

A REST API is a v1 API and is managed by `aws apigateway`. An HTTP API is a v2
API and is managed by `aws apigwv2`, which is covered in
[HTTP APIs](http-apis-v2.md). This file covers v1 only.

## To list REST APIs

The following example lists the id, name, and creation date of every REST API in
the region. The call is paginated, so repeat it with `--position` until the
response has no `position` field.

```bash
aws apigateway get-rest-apis \
    --max-results 500 \
    --query 'items[].[id,name,createdDate]'
```

## To create a REST API

The following example creates a regional API. Use `EDGE` instead for an API
served through CloudFront.

```bash
aws apigateway create-rest-api \
    --name orders-api \
    --description "Orders service" \
    --endpoint-configuration types=REGIONAL
```

## To get one REST API

The following example returns the full API definition, including its resources,
methods, and integrations.

```bash
aws apigateway get-rest-api --rest-api-id <rest-api-id>
```

## To update the description of a REST API

The following example replaces one field of the API definition. A patch
operation is `op`, a JSON pointer in `path`, and a `value`, and several
operations are separated by commas.

```bash
aws apigateway update-rest-api \
    --rest-api-id <rest-api-id> \
    --patch-operations op=replace,path=/description,value="Orders service"
```

## To accept binary payloads

The following example adds a binary media type. A forward slash in a JSON
pointer is escaped as `~1`.

```bash
aws apigateway update-rest-api \
    --rest-api-id <rest-api-id> \
    --patch-operations op=add,path=/binaryMediaTypes/*~1application~1octet-stream
```

## To tag a REST API

The following example adds a tag to the API itself. A stage is tagged as
`restapis/<id>/stages/<stage>`.

```bash
aws apigateway tag-resource \
    --resource-arn arn:aws:apigateway:us-east-1::restapis/<rest-api-id> \
    --tags Key=env,Value=prod
```

## To list the tags of a REST API

The following example lists every tag on the API.

```bash
aws apigateway get-tags \
    --resource-arn arn:aws:apigateway:us-east-1::restapis/<rest-api-id>
```

## To remove a tag

The following example removes the `env` tag and leaves the other tags in place.

```bash
aws apigateway untag-resource \
    --resource-arn arn:aws:apigateway:us-east-1::restapis/<rest-api-id> \
    --tagKeys env
```

## To delete a REST API

The following example deletes an API together with every resource, method,
integration, deployment, and stage under it. There is no dry run, so read the
name back with `get-rest-api` before running this.

```bash
aws apigateway delete-rest-api --rest-api-id <rest-api-id>
```