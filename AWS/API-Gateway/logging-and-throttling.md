# Logging and Throttling

> Logging and Throttling. Part of the [API-Gateway](../) cheatsheet.

Every setting here is per stage, and every stage is edited with
`--patch-operations` on `update-stage`. See
[Deployments and Stages](deployments-and-stages.md).

## To give API Gateway permission to write logs

The following example sets the account-wide CloudWatch role. Until this is set,
no stage can write access logs or execution logs.

```bash
aws apigateway update-account \
    --patch-operations op=replace,path=/cloudWatchRoleArn,value=arn:aws:iam::<account-id>:role/api-gateway-cloudwatch-role
```

## To turn on access logs for a stage

The following example writes one line per request to a log group named after the
stage. `$context` has to be escaped, or the shell expands it.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations 'op=add,path=/accessLogSetting/destinationArn,value=arn:aws:logs:us-east-1:<account-id>:log-group:/aws/apigateway/prod' \
    'op=add,path=/accessLogSetting/format,value=$context.requestId $context.status $context.identity.sourceIp'
```

## To turn on execution logs for a stage

The following example logs every request and response body. Set the level to
`ERROR` in production, because `INFO` logs the payloads.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations op=replace,path=/logging/loglevel,value=INFO
```

## To turn on stage metrics

The following example publishes the `4XXError`, `5XXError`, `Latency`, and
`Count` metrics of the stage to CloudWatch.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations op=replace,path=/metrics/enabled,value=true
```

## To set a throttle on a stage

The following example caps a stage at 100 requests a second with a burst of
200. A per-method throttle in
[API Keys and Usage Plans](api-keys-and-usage-plans.md) overrides this.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations op=replace,path=/throttlingSettings/rateLimit,value=100,op=replace,path=/throttlingSettings/burstLimit,value=200
```

## To cache responses in the method

The following example copies the backend `Cache-Control` header onto the client
response, which requires an API key so one caller cannot poison the cache for
the rest.

```bash
aws apigateway update-method \
    --rest-api-id <rest-api-id> \
    --resource-id <resource-id> \
    --http-method GET \
    --patch-operations op=replace,path=/apiKeyRequired,value=true,op=add,path=/requestParameters/integration.response.header.Cache-Control,value=method.response.header.Cache-Control
```

## To follow the access log

The following example tails the log group the stage writes to, which is the
quickest way to see what a caller actually sent.

```bash
aws logs tail /aws/apigateway/prod --follow
```

## To count errors in a log group

The following example counts the responses that failed, grouped by status.

```bash
aws logs start-query \
    --log-group-name /aws/apigateway/prod \
    --start-time 0 \
    --end-time 9999999999 \
    --query-string 'fields @timestamp, $status | filter $status >= 500 | stats count() by $status'
```