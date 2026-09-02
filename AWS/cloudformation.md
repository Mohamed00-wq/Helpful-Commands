# 🏗️ CloudFormation (Infrastructure as Code)

> Advanced CloudFormation CLI commands for stacks, change sets, StackSets, and templates — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Stack Operations

| Command | Description |
|---|---|
| `aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE` | List active stacks |
| `aws cloudformation describe-stacks --stack-name my-stack` | Get stack details |
| `aws cloudformation create-stack --stack-name my-stack --template-body file://template.json` | Create a stack |
| `aws cloudformation create-stack --stack-name my-stack --template-url https://s3.amazonaws.com/my-bucket/template.json` | Create from S3 template |
| `aws cloudformation create-stack --stack-name my-stack --template-body file://template.json --parameters ParameterKey=Env,ParameterValue=prod` | Create with parameters |
| `aws cloudformation update-stack --stack-name my-stack --template-body file://template-v2.json` | Update a stack |
| `aws cloudformation delete-stack --stack-name my-stack` | Delete a stack |
| `aws cloudformation cancel-update-stack --stack-name my-stack` | Cancel an in-progress update |
| `aws cloudformation describe-stack-events --stack-name my-stack` | View stack events |
| `aws cloudformation get-template --stack-name my-stack` | Get current template |

```bash
# Create stack
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.json \
  --parameters ParameterKey=Env,ParameterValue=prod

# Update stack
aws cloudformation update-stack \
  --stack-name my-stack \
  --template-body file://template-v2.json

# Delete
aws cloudformation delete-stack --stack-name my-stack

# Events (useful for debugging)
aws cloudformation describe-stack-events --stack-name my-stack
```

---

## Stack Resources

| Command | Description |
|---|---|
| `aws cloudformation list-stack-resources --stack-name my-stack` | List all resources in a stack |
| `aws cloudformation describe-stack-resource --stack-name my-stack --logical-resource-id MyEC2Instance` | Get details for a specific resource |
| `aws cloudformation describe-stack-resources --stack-name my-stack --physical-resource-id i-xxxxxxxx` | Find resource by physical ID |
| `aws cloudformation continue-update-rollback --stack-name my-stack` | Continue rollback after failure |

```bash
# List resources
aws cloudformation list-stack-resources --stack-name my-stack

# Continue rollback
aws cloudformation continue-update-rollback --stack-name my-stack
```

---

## Change Sets

| Command | Description |
|---|---|
| `aws cloudformation create-change-set --stack-name my-stack --change-set-name my-changes --template-body file://template-v2.json` | Create a change set |
| `aws cloudformation create-change-set --stack-name my-stack --change-set-name my-changes --template-body file://template.json --change-set-type CREATE` | Create change set for new stack |
| `aws cloudformation describe-change-set --change-set-name my-changes --stack-name my-stack` | View change set details |
| `aws cloudformation list-change-sets --stack-name my-stack` | List change sets |
| `aws cloudformation execute-change-set --change-set-name my-changes --stack-name my-stack` | Execute a change set |
| `aws cloudformation delete-change-set --change-set-name my-changes --stack-name my-stack` | Delete a change set |

```bash
# Create change set
aws cloudformation create-change-set \
  --stack-name my-stack \
  --change-set-name my-changes \
  --template-body file://template-v2.json

# Review changes
aws cloudformation describe-change-set \
  --change-set-name my-changes \
  --stack-name my-stack

# Execute
aws cloudformation execute-change-set \
  --change-set-name my-changes \
  --stack-name my-stack
```

---

## Stack Sets

| Command | Description |
|---|---|
| `aws cloudformation list-stack-sets` | List all stack sets |
| `aws cloudformation describe-stack-set --stack-set-name my-stackset` | Get stack set details |
| `aws cloudformation create-stack-set --stack-set-name my-stackset --template-body file://template.json` | Create a stack set |
| `aws cloudformation create-stack-set --stack-set-name my-stackset --template-body file://template.json --execution-role-name CloudFormationExecutionRole` | Create with execution role |
| `aws cloudformation delete-stack-set --stack-set-name my-stackset` | Delete a stack set |
| `aws cloudformation create-stack-instances --stack-set-name my-stackset --accounts '["123456789012","987654321098"]' --regions '["us-east-1","us-west-2"]'` | Deploy to accounts/regions |
| `aws cloudformation describe-stack-instance --stack-set-name my-stackset --stack-instance-account 123456789012 --stack-instance-region us-east-1` | Get stack instance details |
| `aws cloudformation list-stack-instances --stack-set-name my-stackset` | List all stack instances |
| `aws cloudformation delete-stack-instances --stack-set-name my-stackset --accounts '["123456789012"]' --regions '["us-east-1"]'` | Delete stack instances |
| `aws cloudformation update-stack-instances --stack-set-name my-stackset --accounts '["123456789012"]' --regions '["us-east-1"]' --template-body file://template-v2.json` | Update stack instances |

```bash
# Create stack set
aws cloudformation create-stack-set \
  --stack-set-name my-stackset \
  --template-body file://template.json

# Deploy to multiple accounts/regions
aws cloudformation create-stack-instances \
  --stack-set-name my-stackset \
  --accounts '["123456789012","987654321098"]' \
  --regions '["us-east-1","us-west-2"]'

# List instances
aws cloudformation list-stack-instances --stack-set-name my-stackset
```

---

## Exports & Outputs

| Command | Description |
|---|---|
| `aws cloudformation list-exports` | List all exports |
| `aws cloudformation list-imports --export-name my-export` | Find stacks using an export |

```bash
aws cloudformation list-exports
aws cloudformation list-imports --export-name my-export
```

---

## Validate & Estimate

| Command | Description |
|---|---|
| `aws cloudformation validate-template --template-body file://template.json` | Validate a template |
| `aws cloudformation estimate-template-cost --template-body file://template.json` | Estimate template cost |
| `aws cloudformation get-template-summary --template-body file://template.json` | Get template summary |

```bash
# Validate
aws cloudformation validate-template --template-body file://template.json

# Cost estimate
aws cloudformation estimate-template-cost --template-body file://template.json
```

---

## Drift Detection

| Command | Description |
|---|---|
| `aws cloudformation detect-stack-drift --stack-name my-stack` | Start drift detection |
| `aws cloudformation describe-stack-drift-detection-status --stack-drift-detection-id xxx` | Check drift status |
| `aws cloudformation describe-stack-resource-drifts --stack-name my-stack` | List drifted resources |

```bash
# Detect drift
aws cloudformation detect-stack-drift --stack-name my-stack

# Check status
aws cloudformation describe-stack-drift-detection-status \
  --stack-drift-detection-id xxx

# View drifted resources
aws cloudformation describe-stack-resource-drifts --stack-name my-stack
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
