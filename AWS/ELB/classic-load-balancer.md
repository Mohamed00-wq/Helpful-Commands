# Classic Load Balancer (CLB / ELBv1)

> Classic Load Balancer (CLB / ELBv1). Part of the [ELB](../) cheatsheet.

## To list classic load balancers

## To get details for a specific CLB

The following example gets details for a specific CLB.

```bash
aws elb describe-load-balancers --load-balancer-names my-clb
```

## To create a classic LB

The following example creates a classic LB.

```bash
aws elb create-load-balancer
    --load-balancer-name my-clb
    --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80"
    --subnets subnet-xxxxxxxx
    --security-groups sg-xxxxxxxx
```

## To delete a classic LB

The following example deletes a classic LB.

```bash
aws elb delete-load-balancer --load-balancer-name my-clb
```

## To check CLB instance health

The following example checks CLB instance health.

```bash
aws elb describe-instance-health --load-balancer-name my-clb
```

## To register instances with a CLB

The following example registers instances with a CLB.

```bash
aws elb register-instances-with-load-balancer --load-balancer-name my-clb --instances i-xxxxxxxx
```

## To deregister instances from a CLB

The following example deregisters instances from a CLB.

```bash
aws elb deregister-instances-from-load-balancer --load-balancer-name my-clb --instances i-xxxxxxxx
```

## To view CLB attributes

The following example views CLB attributes.

```bash
aws elb describe-load-balancer-attributes --load-balancer-name my-clb
```

## To configure health check for a CLB

The following example configures health check for a CLB.

```bash
aws elb configure-health-check
    --load-balancer-name my-clb
    --health-check Target=HTTP:80/index.html,Interval=30,Timeout=5,UnhealthyThreshold=2,HealthyThreshold=3
```
