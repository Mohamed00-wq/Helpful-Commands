# Auto Scaling Groups

> Auto Scaling Groups. Part of the [ASG](../ASG.md) cheatsheet.

## To list all ASGs

## To get details for a specific ASG

The following example gets details for a specific auto scaling group.

```bash
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name my-asg
```

## To create an ASG

The following example creates an auto scaling group from a launch template and
spreads it across two subnets.

```bash
aws autoscaling create-auto-scaling-group
    --auto-scaling-group-name my-asg
    --launch-template LaunchTemplateName=my-lt,Version="$LATEST"
    --min-size 1
    --max-size 4
    --desired-capacity 2
    --vpc-zone-identifier "subnet-0123456789abcdef0,subnet-0123456789abcdef1"
```

## To update ASG capacity

The following example updates the size bounds and desired capacity of an auto
scaling group.

```bash
aws autoscaling update-auto-scaling-group
    --auto-scaling-group-name my-asg
    --min-size 2
    --max-size 6
    --desired-capacity 3
```

## To delete an ASG

The following example deletes an auto scaling group. The group must have a
desired capacity of 0 first, otherwise the call fails.

```bash
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg
```

## To force delete an ASG (with instances)

The following example deletes an auto scaling group and terminates its
instances in one call.

```bash
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg --force-delete
```

## To suspend all scaling processes

The following example suspends every scaling process, which is useful before
making a manual change that would otherwise be reverted by a scaling policy.

```bash
aws autoscaling suspend-processes --auto-scaling-group-name my-asg
```

## To resume suspended processes

The following example resumes scaling processes suspended earlier.

```bash
aws autoscaling resume-processes --auto-scaling-group-name my-asg
```

## To view scaling activity history

The following example lists the scaling activities the group has performed,
newest first.

```bash
aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg
```

## To list instances in an ASG

The following example lists the instances currently in the group.

```bash
aws autoscaling describe-auto-scaling-groups
    --auto-scaling-group-name my-asg
    --query "AutoScalingGroups[0].Instances"
```
