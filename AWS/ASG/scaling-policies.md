# Scaling Policies

> Scaling Policies. Part of the [ASG](../ASG.md) cheatsheet.

## To create a target tracking policy (CPU)

The following example creates a target tracking policy (CPU).

```bash
aws autoscaling put-scaling-policy
    --auto-scaling-group-name my-asg
    --policy-name scale-out
    --policy-type TargetTrackingScaling
    --target-tracking-configuration "PredefinedMetricSpecification={PredefinedMetricType=ASGAverageCPUUtilization},TargetValue=70.0"
```

## To create a step scaling policy

The following example creates a step scaling policy.

```bash
aws autoscaling put-scaling-policy
    --auto-scaling-group-name my-asg
    --policy-name scale-out
    --policy-type StepScaling
    --adjustment-type ChangeInCapacity
    --step-adjustments "Magnitude=1"
```

## To create a simple scaling policy

The following example creates a simple scaling policy.

```bash
aws autoscaling put-scaling-policy
    --auto-scaling-group-name my-asg
    --policy-name scale-out
    --policy-type SimpleScaling
    --adjustment-type ChangeInCapacity
    --scaling-adjustment 1
```

## To list scaling policies for an ASG

The following example lists scaling policies for an ASG.

```bash
aws autoscaling describe-policies --auto-scaling-group-name my-asg
```

## To delete a scaling policy

The following example deletes a scaling policy.

```bash
aws autoscaling delete-policy --auto-scaling-group-name my-asg --policy-name scale-out
```
