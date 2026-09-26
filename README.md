# AWS CLI Cheatsheet

A reference collection of AWS CLI commands, written while studying for the AWS
Solutions Architect Associate (SAA-C03) exam. Every file follows the
[AWS CLI code example style guide](https://aws.github.io/aws-cli/docs_styleguide.html):
one command per example, a short description, and a copy-pasteable code block.

## Structure

| Service | File | Covers |
| --- | --- | --- |
| **Compute** | | |
| EC2 | [EC2.md](AWS/EC2.md) | Instances, key pairs, security groups, Elastic IPs, tags, placement groups |
| EC2 Pricing | [EC2-Pricing.md](AWS/EC2-Pricing.md) | Spot, Reserved Instances, Savings Plans, On-Demand |
| Auto Scaling | [ASG.md](AWS/ASG.md) | Launch templates, ASGs, scaling policies, scheduled actions, instance refresh |
| Systems Manager | [SystemsManager.md](AWS/SystemsManager.md) | Sessions, patching, automation runbooks, maintenance windows |
| **Storage** | | |
| S3 | [S3.md](AWS/S3.md) | Buckets, objects, versioning, lifecycle, storage classes, access points |
| EBS | [EBS.md](AWS/EBS.md) | Volumes, snapshots, encryption, resizing, volume types |
| EFS | [EFS.md](AWS/EFS.md) | File systems, mount targets, security groups, access points |
| **Networking** | | |
| VPC | [VPC.md](AWS/VPC.md) | Subnets, NAT, peering, endpoints, NACLs, flow logs, full build workflow |
| ELB | [ELB.md](AWS/ELB.md) | ALB, NLB, CLB, target groups, listeners, rules |
| Route 53 | [Route53.md](AWS/Route53.md) | Hosted zones, record sets, health checks, routing policies |
| CloudFront | [CloudFront.md](AWS/CloudFront.md) | Distributions, invalidations, origin access control, cache policies |
| **Databases** | | |
| RDS | [RDS.md](AWS/RDS.md) | Instances, Aurora, read replicas, snapshots, parameter groups |
| DynamoDB | [DynamoDB.md](AWS/DynamoDB.md) | Tables, items, query, scan, batch operations, indexes, TTL |
| **Containers** | | |
| ECS | [ECS.md](AWS/ECS.md) | Clusters, services, tasks, task definitions, Fargate |
| EKS | [EKS.md](AWS/EKS.md) | Clusters, node groups, Fargate profiles, addons, kubeconfig access |
| ECR | [ECR.md](AWS/ECR.md) | Repositories, images, scanning, replication, Docker login |
| **Serverless and application** | | |
| Lambda | [Lambda.md](AWS/Lambda.md) | Functions, versions, aliases, layers, event sources, concurrency |
| API Gateway | — | *not yet covered* |
| **Messaging** | | |
| SQS and SNS | [SQS-SNS.md](AWS/SQS-SNS.md) | Queues, topics, subscriptions, FIFO, dead letter queues |
| Step Functions | [StepFunctions.md](AWS/StepFunctions.md) | State machines, executions, versions, aliases, redrive |
| **Identity and security** | | |
| IAM | [IAM.md](AWS/IAM.md) | Users, roles, policies, groups, instance profiles, credential reports |
| STS | [STS.md](AWS/STS.md) | Caller identity, assume role, web identity federation |
| KMS and Secrets Manager | [KMS-Secrets.md](AWS/KMS-Secrets.md) | KMS keys, encryption contexts, Secrets Manager, Parameter Store |
| **Operations** | | |
| CloudWatch | [CloudWatch.md](AWS/CloudWatch.md) | Metrics, alarms, logs, dashboards, metric filters |
| CloudTrail | [CloudTrail.md](AWS/CloudTrail.md) | Trails, event data selectors, event history, log file analysis |
| CloudFormation | [CloudFormation.md](AWS/CloudFormation.md) | Stacks, change sets, StackSets, exports, drift detection |
| AMI | [AMI.md](AWS/AMI.md) | Create, copy, share, deregister, block device mappings |
| **Cost** | | |
| Cost Explorer | [CostExplorer.md](AWS/CostExplorer.md) | Cost and usage, dimensions, forecasts, Savings Plans recommendations |

## Quick start

```bash
# Configure the CLI
aws configure

# Check which credentials you are actually using
aws sts get-caller-identity

# List running EC2 instances
aws ec2 describe-instances \
    --filters "Name=instance-state-name,Values=running" \
    --query 'Reservations[].Instances[].[InstanceId,State.Name,PublicIpAddress]' \
    --output table

# Upload a file to S3
aws s3 cp <file> s3://<bucket>/

# Invoke a Lambda function
aws lambda invoke --function-name <function> --payload '{}' out.json
```

## Conventions used in these files

- Commands assume `output` is set to `json` in `~/.aws/config`. The `--output`
  flag appears only in examples that specifically demonstrate another format.
- Destructive commands are paired with a `--dryrun` where the API supports it.
  Where it does not, the file documents the real guard instead.
- List operations that can truncate show pagination flags.
- Placeholders are written in angle brackets: `s3://<bucket>`, `--instance-ids <id>`.
- Multi-step procedures live in a `## Workflows` section at the end of the file.

## Contributing

See [TEMPLATE.md](TEMPLATE.md) for the file structure and
[CONTRIBUTING.md](CONTRIBUTING.md) for the workflow. Run
`npx markdownlint-cli2 "**/*.md"` before opening a pull request.

## License

MIT. See [LICENSE](LICENSE).
