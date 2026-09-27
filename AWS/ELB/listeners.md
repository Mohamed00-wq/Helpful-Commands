# Listeners (ALB / NLB)

> Listeners (ALB / NLB). Part of the [ELB](../ELB.md) cheatsheet.

## To list listeners on a load balancer

The following example lists listeners on a load balancer.

```bash
aws elbv2 describe-listeners --load-balancer-arn arn:...
```

## To create an HTTP listener

The following example creates an HTTP listener.

```bash
aws elbv2 create-listener
    --load-balancer-arn arn:...
    --protocol HTTP
    --port 80
    --default-actions Type=forward,TargetGroupArn=arn:...
```

## To create an HTTPS listener

The following example creates an HTTPS listener.

```bash
aws elbv2 create-listener
    --load-balancer-arn arn:...
    --protocol HTTPS
    --port 443
    --certificates CertificateArn=arn:...
    --default-actions Type=forward,TargetGroupArn=arn:...
```

## To create a TCP listener (NLB)

The following example creates a TCP listener (NLB).

```bash
aws elbv2 create-listener
    --load-balancer-arn arn:...
    --protocol TCP
    --port 80
    --default-actions Type=forward,TargetGroupArn=arn:...
```

## To delete a listener

The following example deletes a listener.

```bash
aws elbv2 delete-listener --listener-arn arn:...
```

## To view certificates on an HTTPS listener

The following example views certificates on an HTTPS listener.

```bash
aws elbv2 describe-listener-certificates --listener-arn arn:...
```

## To add a certificate to an HTTPS listener

The following example adds a certificate to an HTTPS listener.

```bash
aws elbv2 add-listener-certificates --listener-arn arn:... --certificates CertificateArn=arn:...
```
