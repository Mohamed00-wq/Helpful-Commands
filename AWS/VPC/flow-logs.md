# Flow Logs

> Flow Logs. Part of the [VPC](../VPC.md) cheatsheet.

## To list flow logs

The following example lists flow logs.

```bash
aws ec2 describe-flow-logs
```

## To create flow log to CloudWatch

The following example creates flow log to CloudWatch.

```bash
aws ec2 create-flow-logs
    --resource-type VPC
    --resource-ids vpc-xxx
    --traffic-type ALL
    --log-destination-type cloud-watch-logs
    --log-group-name /vpc/flowlogs
    --deliver-logs-permission-arn arn:aws:iam::ACCOUNT:role/FlowLogRole
```

## To create flow log to S3

The following example creates flow log to S3.

```bash
aws ec2 create-flow-logs
    --resource-type VPC
    --resource-ids vpc-xxx
    --traffic-type REJECT
    --log-destination-type s3
    --log-destination arn:aws:s3:::my-bucket/flowlogs/
```

## To delete flow logs

The following example deletes flow logs.

```bash
aws ec2 delete-flow-logs --flow-log-ids fl-xxxxxxxx
```
