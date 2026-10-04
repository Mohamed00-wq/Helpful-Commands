# API Keys and Usage Plans

> API Keys and Usage Plans. Part of the [API-Gateway](../) cheatsheet.

An API key identifies a caller, and a usage plan is what turns that key into a
quota and a throttle. Both are only enforced on methods created with
`--api-key-required true`, or on a whole stage.

## To create an API key

The following example creates a key restricted to one stage. The stage is named
as `<account-id>/<stage-name>`.

```bash
aws apigateway create-api-key \
    --name orders-mobile \
    --description "Mobile client" \
    --enabled true \
    --stage-keys <account-id>/prod
```

## To read the value of an API key

The following example returns the secret value, which is only ever shown here
and at creation time.

```bash
aws apigateway get-api-key \
    --api-key-id <api-key-id> \
    --include-value
```

## To create a usage plan

The following example caps a stage at 100 requests per second and 10000
requests a month.

```bash
aws apigateway create-usage-plan \
    --name orders-standard \
    --api-stages <rest-api-id>/prod \
    --throttle-rate-limit 100 \
    --throttle-burst-limit 200 \
    --quota-limit 10000 \
    --quota-period MONTH
```

## To attach an API key to a usage plan

The following example gives one key the quota of the plan. Without this step the
key can call nothing.

```bash
aws apigateway create-usage-plan-key \
    --usage-plan-id <usage-plan-id> \
    --key-id <api-key-id> \
    --key-type API_KEY
```

## To read a usage plan

The following example returns the plan with its stages, quota, and throttle.

```bash
aws apigateway get-usage-plan --usage-plan-id <usage-plan-id>
```

## To read the usage of a plan

The following example returns the call count over a date range, which is what
the API Gateway usage charts show.

```bash
aws apigateway get-usage \
    --usage-plan-id <usage-plan-id> \
    --start-date 2026-01-01T00:00:00Z \
    --end-date 2026-02-01T00:00:00Z
```

## To turn off an API key

The following example disables a key without deleting it, which is the safe way
to cut off one client.

```bash
aws apigateway update-api-key \
    --api-key-id <api-key-id> \
    --patch-operations op=replace,path=/enabled,value=false
```

## To delete a usage plan

The following example removes a plan. The API keys attached to it stay valid but
lose their quota, so check `get-usage-plan` first.

```bash
aws apigateway delete-usage-plan --usage-plan-id <usage-plan-id>
```