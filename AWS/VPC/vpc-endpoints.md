# VPC Endpoints (PrivateLink)

> VPC Endpoints (PrivateLink). Part of the [VPC](../VPC.md) cheatsheet.

## To list all endpoints

The following example lists all endpoints.

```bash
aws ec2 describe-vpc-endpoints
```

## To create S3 gateway endpoint

The following example creates S3 gateway endpoint.

```bash
aws ec2 create-vpc-endpoint
    --vpc-id vpc-xxx
    --service-name com.amazonaws.us-east-1.s3
    --route-table-ids rtb-xxx
```

## To create DynamoDB endpoint

The following example creates DynamoDB endpoint.

```bash
aws ec2 create-vpc-endpoint
    --vpc-id vpc-xxx
    --service-name com.amazonaws.us-east-1.dynamodb
    --route-table-ids rtb-xxx
```

## To create interface endpoint

The following example creates interface endpoint.

```bash
aws ec2 create-vpc-endpoint
    --vpc-id vpc-xxx
    --service-name com.amazonaws.us-east-1.sqs
    --vpc-endpoint-type Interface
    --subnet-ids subnet-xxx
    --security-group-ids sg-xxx
```

## To delete an endpoint

The following example deletes an endpoint.

```bash
aws ec2 delete-vpc-endpoints --vpc-endpoint-ids vpce-xxxxxxxx
```

## To list available endpoint services

The following example lists available endpoint services.

```bash
aws ec2 describe-vpc-endpoint-services
```
