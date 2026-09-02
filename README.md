# Linux & AWS CLI Cheatsheet

A quick-reference collection of essential Linux and AWS CLI commands, compiled while studying for the AWS Solutions Architect Associate (SAA-C03) certification. Covers networking, system administration, and core AWS services commonly used for troubleshooting and day-to-day cloud operations.

---

## 📂 Structure

### Linux
- `LINUX/commands.md` — Essential Linux commands

### AWS

| Service | File | Topics |
|---------|------|--------|
| EC2 | `AWS/ec2.md` | Instances, key pairs, security groups, Elastic IPs, placement groups |
| EC2 Pricing | `AWS/ec2-pricing.md` | Spot, Reserved Instances, Savings Plans, On-Demand |
| EBS | `AWS/ebs.md` | Volumes, snapshots, encryption, resizing |
| AMI | `AWS/ami.md` | Create, copy, share, block device mappings |
| ELB | `AWS/elb.md` | ALB, NLB, CLB, target groups, listeners, rules |
| IAM | `AWS/iam.md` | Users, roles, policies, groups, STS, MFA, instance profiles |
| ASG | `AWS/asg.md` | Auto scaling groups, launch configs, scaling policies, scheduled actions |
| **S3** | `AWS/s3.md` | Buckets, objects, versioning, lifecycle, replication, access points |
| **RDS** | `AWS/rds.md` | Instances, Aurora, read replicas, snapshots, parameter groups |
| **Lambda** | `AWS/lambda.md` | Functions, layers, versions, event sources, concurrency, function URLs |
| **CloudFormation** | `AWS/cloudformation.md` | Stacks, change sets, StackSets, drift detection |
| **ECS** | `AWS/ecs.md` | Clusters, services, tasks, Fargate, service discovery |
| **EKS** | `AWS/eks.md` | Clusters, node groups, Fargate profiles, addons, access |
| **SQS/SNS** | `AWS/sqs-sns.md` | Queues, topics, subscriptions, FIFO, DLQ, message filtering |
| **CloudWatch** | `AWS/cloudwatch.md` | Alarms, metrics, logs, dashboards, metric filters |
| **VPC** | `AWS/vpc.md` | Subnets, NAT, peering, endpoints, NACLs, flow logs |
| **Route 53** | `AWS/route53.md` | Hosted zones, records, health checks, alias, weighted routing |
| **CloudFront** | `AWS/cloudfront.md` | Distributions, invalidations, OAC, cache policies |
| **KMS/Secrets** | `AWS/kms-secrets.md` | KMS keys, encryption, Secrets Manager, Parameter Store |
| **Systems Manager** | `AWS/systems-manager.md` | Sessions, patching, automation, maintenance windows |
| **VPC Lab** | `AWS/vpc-lab.md` | Step-by-step VPC build from scratch |

---

## 🚀 Quick Start

```bash
# Configure AWS CLI
aws configure

# List all running EC2 instances
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"

# Check current identity
aws sts get-caller-identity

# Upload file to S3
aws s3 cp file.txt s3://my-bucket/

# Invoke Lambda
aws lambda invoke --function-name my-function --payload '{}' output.json
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
