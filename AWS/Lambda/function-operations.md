# Function Operations

> Function Operations. Part of the [Lambda](../Lambda.md) cheatsheet.

## To list all Lambda functions

The following example lists all Lambda functions.

```bash
aws lambda list-functions
```

## To get function details

The following example gets function details.

```bash
aws lambda get-function --function-name my-function
```

## To create a function from zip

The following example creates a function from zip.

```bash
aws lambda create-function
    --function-name my-function
    --runtime python3.12
    --role arn:aws:iam::ACCOUNT:role/lambda-role
    --handler lambda_function.lambda_handler
    --zip-file fileb://function.zip
```

## To create from S3

The following example creates from S3.

```bash
aws lambda create-function
    --function-name my-function
    --runtime python3.12
    --role arn:aws:iam::ACCOUNT:role/lambda-role
    --handler lambda_function.lambda_handler
    --code S3Bucket=my-bucket,S3Key=function.zip
```

## To update function code

The following example updates function code.

```bash
aws lambda update-function-code --function-name my-function --zip-file fileb://function.zip
```

## To update from S3

The following example updates from S3.

```bash
aws lambda update-function-code
    --function-name my-function
    --s3-bucket my-bucket
    --s3-key function-v2.zip
```

## To update config

The following example updates config.

```bash
aws lambda update-function-configuration --function-name my-function --timeout 60 --memory-size 512
```

## To delete a function

The following example deletes a function.

```bash
aws lambda delete-function --function-name my-function
```

## To invoke a function

The following example invokes a function.

```bash
aws lambda invoke --function-name my-function --payload '{"key":"value"}' output.json
```

## To invoke a function asynchronously

The following example invokes a function asynchronously and discards the response.

```bash
aws lambda invoke
    --function-name my-function
    --invocation-type Event
    --payload '{"key":"value"}' /dev/null
```

## To validate a function without running it

The following example validates the function parameters without invoking the function.

```bash
aws lambda invoke --function-name my-function --invocation-type DryRun --payload '{}' /dev/null
```
