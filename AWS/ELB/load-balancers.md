# Load Balancers (ALB / NLB)

> Load Balancers (ALB / NLB). Part of the [ELB](../) cheatsheet.

## To list all ALBs/NLBs

## To get details for a specific LB

The following example gets details for a specific LB.

```bash
aws elbv2 describe-load-balancers --names my-alb
```

## To create an ALB

The following example creates an ALB.

```bash
aws elbv2 create-load-balancer
    --name my-alb
    --subnets subnet-xxxxxxxx subnet-yyyyyyyy
    --security-groups sg-xxxxxxxx
    --type application
```

## To create an NLB

The following example creates an NLB.

```bash
aws elbv2 create-load-balancer
    --name my-nlb
    --subnets subnet-xxxxxxxx subnet-yyyyyyyy
    --type network
```

## To delete a load balancer

The following example deletes a load balancer.

```bash
aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:...
```

## To view LB attributes

The following example views LB attributes.

```bash
aws elbv2 describe-load-balancer-attributes --load-balancer-arn arn:...
```

## To modify LB attributes

The following example modifies LB attributes.

```bash
aws elbv2 modify-load-balancer-attributes
    --load-balancer-arn arn:...
    --attributes Key=idle_timeout.timeout_seconds,Value=60
```

## To view tags on a load balancer

The following example views tags on a load balancer.

```bash
aws elbv2 describe-tags --resource-arns arn:...
```

```bash
# ALB / NLB
aws elbv2 describe-load-balancers
aws elbv2 create-load-balancer \
    --name my-alb \
    --subnets subnet-xxxxxxxx subnet-yyyyyyyy \
    --security-groups sg-xxxxxxxx \
    --type application
aws elbv2 create-load-balancer \
    --name my-nlb \
    --subnets subnet-xxxxxxxx subnet-yyyyyyyy \
    --type network
aws elbv2 delete-load-balancer --load-balancer-arn arn:...
aws elbv2 describe-load-balancer-attributes --load-balancer-arn arn:...
aws elbv2 modify-load-balancer-attributes \
    --load-balancer-arn arn:... \
    --attributes Key=idle_timeout.timeout_seconds,Value=60
```
