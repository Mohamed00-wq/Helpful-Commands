# Target Groups

> Target Groups. Part of the [ELB](../) cheatsheet.

## To list all target groups

## To get details for a specific TG

The following example gets details for a specific TG.

```bash
aws elbv2 describe-target-groups --names my-tg
```

## To create an HTTP target group

The following example creates an HTTP target group.

```bash
aws elbv2 create-target-group --name my-tg --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx
```

## To create a TCP target group (NLB)

The following example creates a TCP target group (NLB).

```bash
aws elbv2 create-target-group --name my-tg --protocol TCP --port 80 --vpc-id vpc-xxxxxxxx
```

## To delete a target group

The following example deletes a target group.

```bash
aws elbv2 delete-target-group --target-group-arn arn:...
```

## To register instances to a TG

The following example registers instances to a TG.

```bash
aws elbv2 register-targets --target-group-arn arn:... --targets Id=i-xxxxxxxx
```

## To deregister instances from a TG

The following example deregisters instances from a TG.

```bash
aws elbv2 deregister-targets --target-group-arn arn:... --targets Id=i-xxxxxxxx
```

## To check target group health

The following example checks target group health.

```bash
aws elbv2 describe-target-health --target-group-arn arn:...
```

## To check health for a specific target

The following example checks health for a specific target.

```bash
aws elbv2 describe-target-health --target-group-arn arn:... --targets Id=i-xxxxxxxx
```

## To modify health check settings

The following example modifies health check settings.

```bash
aws elbv2 modify-target-group
    --target-group-arn arn:...
    --health-check-interval-seconds 10
    --health-check-timeout-seconds 5
```
