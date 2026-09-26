# ELB (Elastic Load Balancing)

> Commands for ALB, NLB, and CLB load balancers, target groups, listeners, and rules.

## Load Balancers (ALB / NLB)

### To list all ALBs/NLBs

### To get details for a specific LB

The following example gets details for a specific LB.

```bash
aws elbv2 describe-load-balancers --names my-alb
```

### To create an ALB

The following example creates an ALB.

```bash
aws elbv2 create-load-balancer
    --name my-alb
    --subnets subnet-xxxxxxxx subnet-yyyyyyyy
    --security-groups sg-xxxxxxxx
    --type application
```

### To create an NLB

The following example creates an NLB.

```bash
aws elbv2 create-load-balancer
    --name my-nlb
    --subnets subnet-xxxxxxxx subnet-yyyyyyyy
    --type network
```

### To delete a load balancer

The following example deletes a load balancer.

```bash
aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:...
```

### To view LB attributes

The following example views LB attributes.

```bash
aws elbv2 describe-load-balancer-attributes --load-balancer-arn arn:...
```

### To modify LB attributes

The following example modifies LB attributes.

```bash
aws elbv2 modify-load-balancer-attributes
    --load-balancer-arn arn:...
    --attributes Key=idle_timeout.timeout_seconds,Value=60
```

### To view tags on a load balancer

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

## Target Groups

### To list all target groups

### To get details for a specific TG

The following example gets details for a specific TG.

```bash
aws elbv2 describe-target-groups --names my-tg
```

### To create an HTTP target group

The following example creates an HTTP target group.

```bash
aws elbv2 create-target-group --name my-tg --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx
```

### To create a TCP target group (NLB)

The following example creates a TCP target group (NLB).

```bash
aws elbv2 create-target-group --name my-tg --protocol TCP --port 80 --vpc-id vpc-xxxxxxxx
```

### To delete a target group

The following example deletes a target group.

```bash
aws elbv2 delete-target-group --target-group-arn arn:...
```

### To register instances to a TG

The following example registers instances to a TG.

```bash
aws elbv2 register-targets --target-group-arn arn:... --targets Id=i-xxxxxxxx
```

### To deregister instances from a TG

The following example deregisters instances from a TG.

```bash
aws elbv2 deregister-targets --target-group-arn arn:... --targets Id=i-xxxxxxxx
```

### To check target group health

The following example checks target group health.

```bash
aws elbv2 describe-target-health --target-group-arn arn:...
```

### To check health for a specific target

The following example checks health for a specific target.

```bash
aws elbv2 describe-target-health --target-group-arn arn:... --targets Id=i-xxxxxxxx
```

### To modify health check settings

The following example modifies health check settings.

```bash
aws elbv2 modify-target-group
    --target-group-arn arn:...
    --health-check-interval-seconds 10
    --health-check-timeout-seconds 5
```

## Listeners (ALB / NLB)

### To list listeners on a load balancer

The following example lists listeners on a load balancer.

```bash
aws elbv2 describe-listeners --load-balancer-arn arn:...
```

### To create an HTTP listener

The following example creates an HTTP listener.

```bash
aws elbv2 create-listener
    --load-balancer-arn arn:...
    --protocol HTTP
    --port 80
    --default-actions Type=forward,TargetGroupArn=arn:...
```

### To create an HTTPS listener

The following example creates an HTTPS listener.

```bash
aws elbv2 create-listener
    --load-balancer-arn arn:...
    --protocol HTTPS
    --port 443
    --certificates CertificateArn=arn:...
    --default-actions Type=forward,TargetGroupArn=arn:...
```

### To create a TCP listener (NLB)

The following example creates a TCP listener (NLB).

```bash
aws elbv2 create-listener
    --load-balancer-arn arn:...
    --protocol TCP
    --port 80
    --default-actions Type=forward,TargetGroupArn=arn:...
```

### To delete a listener

The following example deletes a listener.

```bash
aws elbv2 delete-listener --listener-arn arn:...
```

### To view certificates on an HTTPS listener

The following example views certificates on an HTTPS listener.

```bash
aws elbv2 describe-listener-certificates --listener-arn arn:...
```

### To add a certificate to an HTTPS listener

The following example adds a certificate to an HTTPS listener.

```bash
aws elbv2 add-listener-certificates --listener-arn arn:... --certificates CertificateArn=arn:...
```

## Rules (Path / Host-Based Routing)

### To list rules on a listener

The following example lists rules on a listener.

```bash
aws elbv2 describe-rules --listener-arn arn:...
```

### To create a path-based routing rule

The following example creates a path-based routing rule.

```bash
aws elbv2 create-rule
    --listener-arn arn:...
    --priority 10
    --conditions Field=path-pattern,Values="/api/*"
    --actions Type=forward,TargetGroupArn=arn:...
```

### To create a host-based routing rule

The following example creates a host-based routing rule.

```bash
aws elbv2 create-rule
    --listener-arn arn:...
    --priority 20
    --conditions Field=host-header,Values="api.example.com"
    --actions Type=forward,TargetGroupArn=arn:...
```

### To create a redirect rule

The following example creates a redirect rule.

```bash
aws elbv2 create-rule
    --listener-arn arn:...
    --priority 30
    --conditions Field=path-pattern,Values="/static/*"
    --actions Type=redirect,RedirectConfig="{Protocol=HTTPS,Port=443,StatusCode=HTTP_301}"
```

### To modify an existing rule

The following example modifies an existing rule.

```bash
aws elbv2 modify-rule --rule-arn arn:... --actions Type=forward,TargetGroupArn=arn:...
```

### To delete a rule

The following example deletes a rule.

```bash
aws elbv2 delete-rule --rule-arn arn:...
```

## Classic Load Balancer (CLB / ELBv1)

### To list classic load balancers

### To get details for a specific CLB

The following example gets details for a specific CLB.

```bash
aws elb describe-load-balancers --load-balancer-names my-clb
```

### To create a classic LB

The following example creates a classic LB.

```bash
aws elb create-load-balancer
    --load-balancer-name my-clb
    --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80"
    --subnets subnet-xxxxxxxx
    --security-groups sg-xxxxxxxx
```

### To delete a classic LB

The following example deletes a classic LB.

```bash
aws elb delete-load-balancer --load-balancer-name my-clb
```

### To check CLB instance health

The following example checks CLB instance health.

```bash
aws elb describe-instance-health --load-balancer-name my-clb
```

### To register instances with a CLB

The following example registers instances with a CLB.

```bash
aws elb register-instances-with-load-balancer --load-balancer-name my-clb --instances i-xxxxxxxx
```

### To deregister instances from a CLB

The following example deregisters instances from a CLB.

```bash
aws elb deregister-instances-from-load-balancer --load-balancer-name my-clb --instances i-xxxxxxxx
```

### To view CLB attributes

The following example views CLB attributes.

```bash
aws elb describe-load-balancer-attributes --load-balancer-name my-clb
```

### To configure health check for a CLB

The following example configures health check for a CLB.

```bash
aws elb configure-health-check
    --load-balancer-name my-clb
    --health-check Target=HTTP:80/index.html,Interval=30,Timeout=5,UnhealthyThreshold=2,HealthyThreshold=3
```
