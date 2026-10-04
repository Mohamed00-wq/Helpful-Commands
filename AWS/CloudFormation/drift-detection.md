# Drift Detection

> Drift Detection. Part of the [CloudFormation](../) cheatsheet.

## To start drift detection

The following example starts drift detection.

```bash
aws cloudformation detect-stack-drift --stack-name my-stack
```

## To check drift status

The following example checks drift status.

```bash
aws cloudformation describe-stack-drift-detection-status --stack-drift-detection-id xxx
```

## To list drifted resources

The following example lists drifted resources.

```bash
aws cloudformation describe-stack-resource-drifts --stack-name my-stack
```
