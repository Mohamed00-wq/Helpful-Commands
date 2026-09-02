# ⚡ Lambda (Serverless Functions)

> Advanced Lambda CLI commands for functions, layers, versions, triggers, and deployment — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Function Operations

| Command | Description |
|---|---|
| `aws lambda list-functions` | List all Lambda functions |
| `aws lambda get-function --function-name my-function` | Get function details |
| `aws lambda create-function --function-name my-function --runtime python3.12 --role arn:aws:iam::ACCOUNT:role/lambda-role --handler lambda_function.lambda_handler --zip-file fileb://function.zip` | Create a function from zip |
| `aws lambda create-function --function-name my-function --runtime python3.12 --role arn:aws:iam::ACCOUNT:role/lambda-role --handler lambda_function.lambda_handler --code S3Bucket=my-bucket,S3Key=function.zip` | Create from S3 |
| `aws lambda update-function-code --function-name my-function --zip-file fileb://function.zip` | Update function code |
| `aws lambda update-function-code --function-name my-function --s3-bucket my-bucket --s3-key function-v2.zip` | Update from S3 |
| `aws lambda update-function-configuration --function-name my-function --timeout 60 --memory-size 512` | Update config |
| `aws lambda delete-function --function-name my-function` | Delete a function |
| `aws lambda invoke --function-name my-function --payload '{"key":"value"}' output.json` | Invoke a function |
| `aws lambda invoke --function-name my-function --invocation-type Event --payload '{"key":"value"}' /dev/null` | Async invocation |
| `aws lambda invoke --function-name my-function --invocation-type DryRun --payload '{}' /dev/null` | Dry run (validate only) |

```bash
# Create function
aws lambda create-function \
  --function-name my-function \
  --runtime python3.12 \
  --role arn:aws:iam::ACCOUNT:role/lambda-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip

# Update code
aws lambda update-function-code \
  --function-name my-function \
  --zip-file fileb://function.zip

# Invoke
aws lambda invoke \
  --function-name my-function \
  --payload '{"key":"value"}' \
  output.json
```

---

## Versions & Aliases

| Command | Description |
|---|---|
| `aws lambda list-versions-by-function --function-name my-function` | List all versions |
| `aws lambda publish-version --function-name my-function` | Publish a new version |
| `aws lambda publish-version --function-name my-function --description "v2 release"` | Publish with description |
| `aws lambda list-aliases --function-name my-function` | List aliases |
| `aws lambda create-alias --function-name my-function --name PROD --function-version 2` | Create an alias |
| `aws lambda update-alias --function-name my-function --name PROD --function-version 3` | Update an alias |
| `aws lambda delete-alias --function-name my-function --name PROD` | Delete an alias |
| `aws lambda get-function --function-name my-function --qualifier PROD` | Get specific version/alias |

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

---

## Layers

| Command | Description |
|---|---|
| `aws lambda list-layers` | List all layers |
| `aws lambda publish-layer-version --layer-name my-layer --zip-file fileb://layer.zip --compatible-runtimes python3.12` | Publish a layer version |
| `aws lambda get-layer-version --layer-name my-layer --version-number 1` | Get layer version details |
| `aws lambda list-layer-versions --layer-name my-layer` | List layer versions |
| `aws lambda delete-layer-version --layer-name my-layer --version-number 1` | Delete a layer version |
| `aws lambda update-function-configuration --function-name my-function --layers arn:aws:lambda:us-east-1:ACCOUNT:layer:my-layer:1` | Attach a layer |

```bash
# Publish layer
aws lambda publish-layer-version \
  --layer-name my-layer \
  --zip-file fileb://layer.zip \
  --compatible-runtimes python3.12

# Attach to function
aws lambda update-function-configuration \
  --function-name my-function \
  --layers arn:aws:lambda:us-east-1:ACCOUNT:layer:my-layer:1
```

---

## Event Source Mapping

| Command | Description |
|---|---|
| `aws lambda list-event-source-mappings --function-name my-function` | List event source mappings |
| `aws lambda create-event-source-mapping --function-name my-function --event-source-arn arn:aws:sqs:us-east-1:ACCOUNT:my-queue --batch-size 10` | Map SQS queue |
| `aws lambda create-event-source-mapping --function-name my-function --event-source-arn arn:aws:kinesis:us-east-1:ACCOUNT:stream/my-stream --starting-position LATEST` | Map Kinesis stream |
| `aws lambda create-event-source-mapping --function-name my-function --event-source-arn arn:aws:dynamodb:us-east-1:ACCOUNT:table/my-table/stream/xxx --starting-position LATEST` | Map DynamoDB stream |
| `aws lambda update-event-source-mapping --uuid xxx --batch-size 50` | Update mapping config |
| `aws lambda delete-event-source-mapping --uuid xxx` | Delete event source mapping |

```bash
# SQS trigger
aws lambda create-event-source-mapping \
  --function-name my-function \
  --event-source-arn arn:aws:sqs:us-east-1:ACCOUNT:my-queue \
  --batch-size 10

# Kinesis trigger
aws lambda create-event-source-mapping \
  --function-name my-function \
  --event-source-arn arn:aws:kinesis:us-east-1:ACCOUNT:stream/my-stream \
  --starting-position LATEST
```

---

## Concurrency & Throttling

| Command | Description |
|---|---|
| `aws lambda get-function-concurrency --function-name my-function` | Get reserved concurrency |
| `aws lambda put-function-concurrency --function-name my-function --reserved-concurrent-executions 100` | Set reserved concurrency |
| `aws lambda delete-function-concurrency --function-name my-function` | Remove reserved concurrency |
| `aws lambda put-provisioned-concurrency-config --function-name my-function --qualifier PROD --provisioned-concurrent-executions 10` | Set provisioned concurrency |
| `aws lambda get-provisioned-concurrency-config --function-name my-function --qualifier PROD` | Get provisioned concurrency |
| `aws lambda delete-provisioned-concurrency-config --function-name my-function --qualifier PROD` | Remove provisioned concurrency |

```bash
# Reserved concurrency
aws lambda put-function-concurrency \
  --function-name my-function \
  --reserved-concurrent-executions 100

# Provisioned concurrency
aws lambda put-provisioned-concurrency-config \
  --function-name my-function \
  --qualifier PROD \
  --provisioned-concurrent-executions 10
```

---

## Permissions & VPC

| Command | Description |
|---|---|
| `aws lambda add-permission --function-name my-function --statement-id s3-trigger --principal s3.amazonaws.com --action lambda:InvokeFunction --source-arn arn:aws:s3:::my-bucket --source-account 123456789012` | Add a resource-based policy |
| `aws lambda remove-permission --function-name my-function --statement-id s3-trigger` | Remove a permission |
| `aws lambda get-policy --function-name my-function` | Get function policy |
| `aws lambda update-function-configuration --function-name my-function --vpc-config SubnetIds=subnet-xxxxxxxx,SecurityGroupIds=sg-xxxxxxxx` | Attach to VPC |
| `aws lambda update-function-configuration --function-name my-function --vpc-config ""` | Remove from VPC |

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

---

## Function URLs

| Command | Description |
|---|---|
| `aws lambda create-function-url-config --function-name my-function --auth-type NONE` | Create a public function URL |
| `aws lambda create-function-url-config --function-name my-function --auth-type AWS_IAM` | Create an IAM-protected URL |
| `aws lambda list-function-url-configs --function-name my-function` | List function URLs |
| `aws lambda get-function-url-config --function-name my-function` | Get function URL details |
| `aws lambda delete-function-url-config --function-name my-function` | Delete function URL |

```bash
aws lambda create-function-url-config \
  --function-name my-function \
  --auth-type NONE
```

---

## Tags

| Command | Description |
|---|---|
| `aws lambda list-tags --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function` | List tags |
| `aws lambda tag-resource --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function --tags env=prod,team=backend` | Add tags |
| `aws lambda untag-resource --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function --tag-keys env` | Remove tags |

```bash
aws lambda tag-resource \
  --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function \
  --tags env=prod,team=backend
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
