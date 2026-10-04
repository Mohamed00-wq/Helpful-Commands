# AWS CLI Cheatsheet

AWS CLI commands, stored so you can find one with a search instead of reading a
long document. Every command lives in its own small file under `AWS/`, so a
search returns a few lines rather than a whole service dump.

Written while studying for the AWS Solutions Architect Associate (SAA-C03) exam,
following the
[AWS CLI code example style guide](https://aws.github.io/aws-cli/docs_styleguide.html).

## Layout

One folder per service, one resource group per file:

```text
AWS/S3/buckets.md
AWS/S3/objects.md
AWS/DynamoDB/query-and-scan.md
```

`ls AWS/*/` lists every file.

## Search

With [ripgrep](https://github.com/BurntSushi/ripgrep):

```bash
# Search every command, description and heading
rg 'put-item'

# Search inside one service
rg 'condition' AWS/DynamoDB/

# Which files mention a flag
rg -l -- '--deletion-protection'

# Show a command with the sentence that describes it
rg -B2 'describe-instances'

# Search one service and open the first hit
less "$(rg -l 'put-item' AWS/DynamoDB/ | head -1)"
```

Without ripgrep, `grep` does the same job:

```bash
grep -rn 'put-item' AWS/
grep -rln -- '--deletion-protection' AWS/
```

## Services

`AWS/` has one folder per service:

```text
AMI          API-Gateway  ASG          CloudFormation  CloudFront
CloudTrail   CloudWatch   CostExplorer DynamoDB        EBS
EC2          EC2-Pricing  ECR          ECS             EFS
EKS          ELB          IAM          KMS-Secrets     Lambda
RDS          Route53      S3           SQS-SNS         StepFunctions
STS          SystemsManager            VPC
```

## Conventions used in these files

- One command per code block, with one prose sentence above it. Continuations
  use `\` indented four spaces.
- No `--output` flag unless the example is demonstrating another output format.
  Set `output = json` once in `~/.aws/config` instead.
- Placeholders are written in angle brackets: `s3://<bucket>`,
  `--instance-ids <id>`.
- Destructive commands are paired with a `--dryrun` where the API supports it.
  Where it does not, the file documents the real guard instead.
- List operations that can truncate show pagination flags.

## License

MIT. See [LICENSE](LICENSE).