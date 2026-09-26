# Lambda (Serverless Functions)

> Commands for functions, versions, aliases, layers, event sources, and concurrency.

## Function Operations

### To list all Lambda functions

The following example lists all Lambda functions.

```bash
aws lambda list-functions
```

### To get function details

The following example gets function details.

```bash
aws lambda get-function --function-name my-function
```

### To create a function from zip

The following example creates a function from zip.

```bash
aws lambda create-function
    --function-name my-function
    --runtime python3.12
    --role arn:aws:iam::ACCOUNT:role/lambda-role
    --handler lambda_function.lambda_handler
    --zip-file fileb://function.zip
```

### To create from S3

The following example creates from S3.

```bash
aws lambda create-function
    --function-name my-function
    --runtime python3.12
    --role arn:aws:iam::ACCOUNT:role/lambda-role
    --handler lambda_function.lambda_handler
    --code S3Bucket=my-bucket,S3Key=function.zip
```

### To update function code

The following example updates function code.

```bash
aws lambda update-function-code --function-name my-function --zip-file fileb://function.zip
```

### To update from S3

The following example updates from S3.

```bash
aws lambda update-function-code
    --function-name my-function
    --s3-bucket my-bucket
    --s3-key function-v2.zip
```

### To update config

The following example updates config.

```bash
aws lambda update-function-configuration --function-name my-function --timeout 60 --memory-size 512
```

### To delete a function

The following example deletes a function.

```bash
aws lambda delete-function --function-name my-function
```

### To invoke a function

The following example invokes a function.

```bash
aws lambda invoke --function-name my-function --payload '{"key":"value"}' output.json
```

### To invoke a function asynchronously

The following example invokes a function asynchronously and discards the response.

```bash
aws lambda invoke
    --function-name my-function
    --invocation-type Event
    --payload '{"key":"value"}' /dev/null
```

### To validate a function without running it

The following example validates the function parameters without invoking the function.

```bash
aws lambda invoke --function-name my-function --invocation-type DryRun --payload '{}' /dev/null
```

## Versions & Aliases

### To list all versions

The following example lists all versions.

```bash
aws lambda list-versions-by-function --function-name my-function
```

### To publish a new version

The following example publishes a new version.

```bash
aws lambda publish-version --function-name my-function
```

### To publish with description

The following example publishes with description.

```bash
aws lambda publish-version --function-name my-function --description "v2 release"
```

### To list aliases

The following example lists aliases.

```bash
aws lambda list-aliases --function-name my-function
```

### To create an alias

The following example creates an alias.

```bash
aws lambda create-alias --function-name my-function --name PROD --function-version 2
```

### To update an alias

The following example updates an alias.

```bash
aws lambda update-alias --function-name my-function --name PROD --function-version 3
```

### To delete an alias

The following example deletes an alias.

```bash
aws lambda delete-alias --function-name my-function --name PROD
```

### To get specific version/alias

The following example gets specific version/alias.

```bash
aws lambda get-function --function-name my-function --qualifier PROD
```

```bash
# Publish version
aws lambda publish-version --function-name my-function

# Create alias
aws lambda create-alias \
    --function-name my-function \
    --name PROD \
    --function-version 2

# Invoke specific version
aws lambda invoke \
    --function-name my-function:PROD \
    --payload '{}' output.json
```

## Layers

### To list all layers

The following example lists all layers.

```bash
aws lambda list-layers
```

### To publish a layer version

The following example publishes a layer version.

```bash
aws lambda publish-layer-version
    --layer-name my-layer
    --zip-file fileb://layer.zip
    --compatible-runtimes python3.12
```

### To get layer version details

The following example gets layer version details.

```bash
aws lambda get-layer-version --layer-name my-layer --version-number 1
```

### To list layer versions

The following example lists layer versions.

```bash
aws lambda list-layer-versions --layer-name my-layer
```

### To delete a layer version

The following example deletes a layer version.

```bash
aws lambda delete-layer-version --layer-name my-layer --version-number 1
```

### To attach a layer

The following example attaches a layer.

```bash
aws lambda update-function-configuration
    --function-name my-function
    --layers arn:aws:lambda:us-east-1:ACCOUNT:layer:my-layer:1
```

## Event Source Mapping

### To list event source mappings

The following example lists event source mappings.

```bash
aws lambda list-event-source-mappings --function-name my-function
```

### To map SQS queue

The following example maps SQS queue.

```bash
aws lambda create-event-source-mapping
    --function-name my-function
    --event-source-arn arn:aws:sqs:us-east-1:ACCOUNT:my-queue
    --batch-size 10
```

### To map Kinesis stream

The following example maps Kinesis stream.

```bash
aws lambda create-event-source-mapping
    --function-name my-function
    --event-source-arn arn:aws:kinesis:us-east-1:ACCOUNT:stream/my-stream
    --starting-position LATEST
```

### To map DynamoDB stream

The following example maps DynamoDB stream.

```bash
aws lambda create-event-source-mapping
    --function-name my-function
    --event-source-arn arn:aws:dynamodb:us-east-1:ACCOUNT:table/my-table/stream/xxx
    --starting-position LATEST
```

### To update mapping config

The following example updates mapping config.

```bash
aws lambda update-event-source-mapping --uuid xxx --batch-size 50
```

### To delete event source mapping

The following example deletes event source mapping.

```bash
aws lambda delete-event-source-mapping --uuid xxx
```

## Concurrency & Throttling

### To get reserved concurrency

The following example gets reserved concurrency.

```bash
aws lambda get-function-concurrency --function-name my-function
```

### To set reserved concurrency

The following example sets reserved concurrency.

```bash
aws lambda put-function-concurrency --function-name my-function --reserved-concurrent-executions 100
```

### To remove reserved concurrency

The following example removes reserved concurrency.

```bash
aws lambda delete-function-concurrency --function-name my-function
```

### To set provisioned concurrency

The following example sets provisioned concurrency.

```bash
aws lambda put-provisioned-concurrency-config
    --function-name my-function
    --qualifier PROD
    --provisioned-concurrent-executions 10
```

### To get provisioned concurrency

The following example gets provisioned concurrency.

```bash
aws lambda get-provisioned-concurrency-config --function-name my-function --qualifier PROD
```

### To remove provisioned concurrency

The following example removes provisioned concurrency.

```bash
aws lambda delete-provisioned-concurrency-config --function-name my-function --qualifier PROD
```

## Permissions & VPC

### To add a resource-based policy

The following example adds a resource-based policy.

```bash
aws lambda add-permission
    --function-name my-function
    --statement-id s3-trigger
    --principal s3.amazonaws.com
    --action lambda:InvokeFunction
    --source-arn arn:aws:s3:::my-bucket
    --source-account 123456789012
```

### To remove a permission

The following example removes a permission.

```bash
aws lambda remove-permission --function-name my-function --statement-id s3-trigger
```

### To get function policy

The following example gets function policy.

```bash
aws lambda get-policy --function-name my-function
```

### To attach to VPC

The following example attaches to VPC.

```bash
aws lambda update-function-configuration
    --function-name my-function
    --vpc-config SubnetIds=subnet-xxxxxxxx,SecurityGroupIds=sg-xxxxxxxx
```

### To remove from VPC

The following example removes from VPC.

```bash
aws lambda update-function-configuration --function-name my-function --vpc-config ""
```

```bash
# Add S3 trigger permission
aws lambda add-permission \
    --function-name my-function \
    --statement-id s3-trigger \
    --principal s3.amazonaws.com \
    --action lambda:InvokeFunction \
    --source-arn arn:aws:s3:::my-bucket

# Attach to VPC
aws lambda update-function-configuration \
    --function-name my-function \
    --vpc-config SubnetIds=subnet-xxxxxxxx,SecurityGroupIds=sg-xxxxxxxx
```

## Function URLs

### To create a public function URL

The following example creates a public function URL.

```bash
aws lambda create-function-url-config --function-name my-function --auth-type NONE
```

### To create an IAM-protected URL

The following example creates an IAM-protected URL.

```bash
aws lambda create-function-url-config --function-name my-function --auth-type AWS_IAM
```

### To list function URLs

The following example lists function URLs.

```bash
aws lambda list-function-url-configs --function-name my-function
```

### To get function URL details

The following example gets function URL details.

```bash
aws lambda get-function-url-config --function-name my-function
```

### To delete function URL

The following example deletes function URL.

```bash
aws lambda delete-function-url-config --function-name my-function
```

## Tags

### To list tags

The following example lists tags.

```bash
aws lambda list-tags --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function
```

### To add tags

The following example adds tags.

```bash
aws lambda tag-resource
    --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function
    --tags env=prod,team=backend
```

### To remove tags

The following example removes tags.

```bash
aws lambda untag-resource
    --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function
    --tag-keys env
```
