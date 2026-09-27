# Permissions & VPC

> Permissions & VPC. Part of the [Lambda](../Lambda.md) cheatsheet.

## To add a resource-based policy

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

## To remove a permission

The following example removes a permission.

```bash
aws lambda remove-permission --function-name my-function --statement-id s3-trigger
```

## To get function policy

The following example gets function policy.

```bash
aws lambda get-policy --function-name my-function
```

## To attach to VPC

The following example attaches to VPC.

```bash
aws lambda update-function-configuration
    --function-name my-function
    --vpc-config SubnetIds=subnet-xxxxxxxx,SecurityGroupIds=sg-xxxxxxxx
```

## To remove from VPC

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
