# ⚖️ Elastic Load Balancing (ELB)

> Essential ELB CLI commands for ALB, NLB, CLB, target groups, listeners, and routing — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Load Balancers (ALB / NLB)

| Command | Description |
|---|---|
| `aws elbv2 describe-load-balancers` | List all ALBs/NLBs |
| `aws elbv2 describe-load-balancers --names my-alb` | Get details for a specific LB |
| `aws elbv2 create-load-balancer --name my-alb --subnets subnet-xxxxxxxx subnet-yyyyyyyy --security-groups sg-xxxxxxxx --type application` | Create an ALB |
| `aws elbv2 create-load-balancer --name my-nlb --subnets subnet-xxxxxxxx subnet-yyyyyyyy --type network` | Create an NLB |
| `aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:...` | Delete a load balancer |
| `aws elbv2 describe-load-balancer-attributes --load-balancer-arn arn:...` | View LB attributes |
| `aws elbv2 modify-load-balancer-attributes --load-balancer-arn arn:... --attributes Key=idle_timeout.timeout_seconds,Value=60` | Modify LB attributes |
| `aws elbv2 describe-tags --resource-arns arn:...` | View tags on a load balancer |

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

---

## Target Groups

| Command | Description |
|---|---|
| `aws elbv2 describe-target-groups` | List all target groups |
| `aws elbv2 describe-target-groups --names my-tg` | Get details for a specific TG |
| `aws elbv2 create-target-group --name my-tg --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx` | Create an HTTP target group |
| `aws elbv2 create-target-group --name my-tg --protocol TCP --port 80 --vpc-id vpc-xxxxxxxx` | Create a TCP target group (NLB) |
| `aws elbv2 delete-target-group --target-group-arn arn:...` | Delete a target group |
| `aws elbv2 register-targets --target-group-arn arn:... --targets Id=i-xxxxxxxx` | Register instances to a TG |
| `aws elbv2 deregister-targets --target-group-arn arn:... --targets Id=i-xxxxxxxx` | Deregister instances from a TG |
| `aws elbv2 describe-target-health --target-group-arn arn:...` | Check target group health |
| `aws elbv2 describe-target-health --target-group-arn arn:... --targets Id=i-xxxxxxxx` | Check health for a specific target |
| `aws elbv2 modify-target-group --target-group-arn arn:... --health-check-interval-seconds 10 --health-check-timeout-seconds 5` | Modify health check settings |

```bash
# Target Groups
aws elbv2 describe-target-groups
aws elbv2 create-target-group \
  --name my-tg --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx
aws elbv2 create-target-group \
  --name my-tg --protocol TCP --port 80 --vpc-id vpc-xxxxxxxx
aws elbv2 delete-target-group --target-group-arn arn:...

# Register / Deregister
aws elbv2 register-targets \
  --target-group-arn arn:... --targets Id=i-xxxxxxxx
aws elbv2 deregister-targets \
  --target-group-arn arn:... --targets Id=i-xxxxxxxx

# Health Checks
aws elbv2 describe-target-health --target-group-arn arn:...
aws elbv2 modify-target-group \
  --target-group-arn arn:... \
  --health-check-interval-seconds 10 \
  --health-check-timeout-seconds 5
```

---

## Listeners (ALB / NLB)

| Command | Description |
|---|---|
| `aws elbv2 describe-listeners --load-balancer-arn arn:...` | List listeners on a load balancer |
| `aws elbv2 create-listener --load-balancer-arn arn:... --protocol HTTP --port 80 --default-actions Type=forward,TargetGroupArn=arn:...` | Create an HTTP listener |
| `aws elbv2 create-listener --load-balancer-arn arn:... --protocol HTTPS --port 443 --certificates CertificateArn=arn:... --default-actions Type=forward,TargetGroupArn=arn:...` | Create an HTTPS listener |
| `aws elbv2 create-listener --load-balancer-arn arn:... --protocol TCP --port 80 --default-actions Type=forward,TargetGroupArn=arn:...` | Create a TCP listener (NLB) |
| `aws elbv2 delete-listener --listener-arn arn:...` | Delete a listener |
| `aws elbv2 describe-listener-certificates --listener-arn arn:...` | View certificates on an HTTPS listener |
| `aws elbv2 add-listener-certificates --listener-arn arn:... --certificates CertificateArn=arn:...` | Add a certificate to an HTTPS listener |

```bash
# Listeners
aws elbv2 describe-listeners --load-balancer-arn arn:...
aws elbv2 create-listener \
  --load-balancer-arn arn:... \
  --protocol HTTP --port 80 \
  --default-actions Type=forward,TargetGroupArn=arn:...
aws elbv2 create-listener \
  --load-balancer-arn arn:... \
  --protocol HTTPS --port 443 \
  --certificates CertificateArn=arn:... \
  --default-actions Type=forward,TargetGroupArn=arn:...
aws elbv2 delete-listener --listener-arn arn:...
```

---

## Rules (Path / Host-Based Routing)

| Command | Description |
|---|---|
| `aws elbv2 describe-rules --listener-arn arn:...` | List rules on a listener |
| `aws elbv2 create-rule --listener-arn arn:... --priority 10 --conditions Field=path-pattern,Values="/api/*" --actions Type=forward,TargetGroupArn=arn:...` | Create a path-based routing rule |
| `aws elbv2 create-rule --listener-arn arn:... --priority 20 --conditions Field=host-header,Values="api.example.com" --actions Type=forward,TargetGroupArn=arn:...` | Create a host-based routing rule |
| `aws elbv2 create-rule --listener-arn arn:... --priority 30 --conditions Field=path-pattern,Values="/static/*" --actions Type=redirect,RedirectConfig="{Protocol=HTTPS,Port=443,StatusCode=HTTP_301}"` | Create a redirect rule |
| `aws elbv2 modify-rule --rule-arn arn:... --actions Type=forward,TargetGroupArn=arn:...` | Modify an existing rule |
| `aws elbv2 delete-rule --rule-arn arn:...` | Delete a rule |

```bash
# Rules
aws elbv2 describe-rules --listener-arn arn:...
aws elbv2 create-rule \
  --listener-arn arn:... --priority 10 \
  --conditions Field=path-pattern,Values="/api/*" \
  --actions Type=forward,TargetGroupArn=arn:...
aws elbv2 create-rule \
  --listener-arn arn:... --priority 20 \
  --conditions Field=host-header,Values="api.example.com" \
  --actions Type=forward,TargetGroupArn=arn:...
aws elbv2 delete-rule --rule-arn arn:...
```

---

## Classic Load Balancer (CLB / ELBv1)

| Command | Description |
|---|---|
| `aws elb describe-load-balancers` | List classic load balancers |
| `aws elb describe-load-balancers --load-balancer-names my-clb` | Get details for a specific CLB |
| `aws elb create-load-balancer --load-balancer-name my-clb --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80" --subnets subnet-xxxxxxxx --security-groups sg-xxxxxxxx` | Create a classic LB |
| `aws elb delete-load-balancer --load-balancer-name my-clb` | Delete a classic LB |
| `aws elb describe-instance-health --load-balancer-name my-clb` | Check CLB instance health |
| `aws elb register-instances-with-load-balancer --load-balancer-name my-clb --instances i-xxxxxxxx` | Register instances with a CLB |
| `aws elb deregister-instances-from-load-balancer --load-balancer-name my-clb --instances i-xxxxxxxx` | Deregister instances from a CLB |
| `aws elb describe-load-balancer-attributes --load-balancer-name my-clb` | View CLB attributes |
| `aws elb configure-health-check --load-balancer-name my-clb --health-check Target=HTTP:80/index.html,Interval=30,Timeout=5,UnhealthyThreshold=2,HealthyThreshold=3` | Configure health check for a CLB |

```bash
# Classic ELB
aws elb describe-load-balancers
aws elb create-load-balancer \
  --load-balancer-name my-clb \
  --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80" \
  --subnets subnet-xxxxxxxx \
  --security-groups sg-xxxxxxxx
aws elb describe-instance-health --load-balancer-name my-clb
aws elb register-instances-with-load-balancer \
  --load-balancer-name my-clb --instances i-xxxxxxxx
aws elb deregister-instances-from-load-balancer \
  --load-balancer-name my-clb --instances i-xxxxxxxx
aws elb configure-health-check \
  --load-balancer-name my-clb \
  --health-check Target=HTTP:80/index.html,Interval=30,Timeout=5,UnhealthyThreshold=2,HealthyThreshold=3
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
