# Rules (Path / Host-Based Routing)

> Rules (Path / Host-Based Routing). Part of the [ELB](../ELB.md) cheatsheet.

## To list rules on a listener

The following example lists rules on a listener.

```bash
aws elbv2 describe-rules --listener-arn arn:...
```

## To create a path-based routing rule

The following example creates a path-based routing rule.

```bash
aws elbv2 create-rule
    --listener-arn arn:...
    --priority 10
    --conditions Field=path-pattern,Values="/api/*"
    --actions Type=forward,TargetGroupArn=arn:...
```

## To create a host-based routing rule

The following example creates a host-based routing rule.

```bash
aws elbv2 create-rule
    --listener-arn arn:...
    --priority 20
    --conditions Field=host-header,Values="api.example.com"
    --actions Type=forward,TargetGroupArn=arn:...
```

## To create a redirect rule

The following example creates a redirect rule.

```bash
aws elbv2 create-rule
    --listener-arn arn:...
    --priority 30
    --conditions Field=path-pattern,Values="/static/*"
    --actions Type=redirect,RedirectConfig="{Protocol=HTTPS,Port=443,StatusCode=HTTP_301}"
```

## To modify an existing rule

The following example modifies an existing rule.

```bash
aws elbv2 modify-rule --rule-arn arn:... --actions Type=forward,TargetGroupArn=arn:...
```

## To delete a rule

The following example deletes a rule.

```bash
aws elbv2 delete-rule --rule-arn arn:...
```
