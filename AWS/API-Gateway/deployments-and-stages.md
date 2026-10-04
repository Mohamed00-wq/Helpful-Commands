# Deployments and Stages

> Deployments and Stages. Part of the [API-Gateway](../) cheatsheet.

A deployment is a snapshot of the API definition, and a stage is a named
pointer at one deployment plus its own settings.

## To create a deployment and a stage in one call

The following example snapshots the API and exposes it as `prod`. The
`version` variable is available to the mappings of that stage.

```bash
aws apigateway create-deployment \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --description "Release 42" \
    --variables version=v42
```

## To deploy to an existing stage

The following example points `prod` at a new deployment. Stage settings such as
caching and logging are kept.

```bash
aws apigateway create-deployment \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --description "Release 43"
```

## To list deployments

The following example lists the deployments of an API, newest first.

```bash
aws apigateway get-deployments --rest-api-id <rest-api-id>
```

## To inspect one deployment

The following example returns a single deployment, which is how you find the
id to roll back to.

```bash
aws apigateway get-deployment \
    --rest-api-id <rest-api-id> \
    --deployment-id <deployment-id>
```

## To roll back to an earlier deployment

The following example points the stage back at a previous deployment. Nothing
is redeployed, so the rollback is immediate.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations op=replace,path=/deploymentId,value=<deployment-id>
```

## To create a second stage

The following example creates `staging` from an existing deployment instead of
deploying again.

```bash
aws apigateway create-stage \
    --rest-api-id <rest-api-id> \
    --deployment-id <deployment-id> \
    --stage-name staging \
    --variables version=v42 \
    --tags Key=env,Value=staging
```

## To list stages

The following example lists every stage of an API with its deployment id.

```bash
aws apigateway get-stages --rest-api-id <rest-api-id>
```

## To read one stage

The following example returns the settings of a single stage.

```bash
aws apigateway get-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod
```

## To change a stage variable

The following example replaces one variable and leaves the others alone.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations op=replace,path=/variables/version,value=v43
```

## To turn on tracing for a stage

The following example enables X-Ray tracing. The function behind the stage
needs the `AWSXRayDaemonWriteAccess` permission as well.

```bash
aws apigateway update-stage \
    --rest-api-id <rest-api-id> \
    --stage-name prod \
    --patch-operations op=replace,path=/tracingEnabled,value=true
```

## To delete a stage

The following example removes a stage. Its deployments stay in the list, so
nothing is lost. There is no dry run.

```bash
aws apigateway delete-stage \
    --rest-api-id <rest-api-id> \
    --stage-name staging
```