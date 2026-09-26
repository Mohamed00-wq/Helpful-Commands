# ASG (Auto Scaling Groups)

> Commands for launch templates, auto scaling groups, scaling policies, and
> scheduled actions.

Auto scaling groups launch instances from a **launch template**. Launch
configurations are deprecated: accounts created on or after 1 October 2024
cannot create one by any method, including the CLI.

## Launch Templates

### To create a launch template

The following example creates a launch template version that an auto scaling
group can launch instances from.

```bash
aws ec2 create-launch-template
    --launch-template-name my-lt
    --launch-template-data '{"ImageId":"ami-0123456789abcdef0","InstanceType":"t3.micro","KeyName":"my-key","SecurityGroupIds":["sg-0123456789abcdef0"]}'
```

### To create a launch template version

The following example publishes a new version of an existing launch template
without changing the default version.

```bash
aws ec2 create-launch-template-version
    --launch-template-name my-lt
    --launch-template-data '{"InstanceType":"t3.small"}'
    --version-description "upgraded instance type"
```

### To set a launch template version as the default

The following example marks a specific version as the default for new launches.

```bash
aws ec2 modify-launch-template
    --launch-template-name my-lt
    --set-default-version 2
```

### To list launch templates

The following example lists every launch template in the account.

```bash
aws ec2 describe-launch-templates
```

### To get launch template versions

The following example lists the versions of a launch template and which one is
the default.

```bash
aws ec2 describe-launch-template-versions --launch-template-name my-lt
```

### To get a launch template version

The following example retrieves one version, including the instance data that
an auto scaling group will use.

```bash
aws ec2 describe-launch-template-versions
    --launch-template-name my-lt
    --versions 1
```

### To delete a launch template

The following example deletes a launch template. It fails if the template is
still referenced by an auto scaling group.

```bash
aws ec2 delete-launch-template --launch-template-name my-lt
```

## Auto Scaling Groups

### To list all ASGs

### To get details for a specific ASG

The following example gets details for a specific auto scaling group.

```bash
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name my-asg
```

### To create an ASG

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

### To update ASG capacity

The following example updates the size bounds and desired capacity of an auto
scaling group.

```bash
aws autoscaling update-auto-scaling-group
    --auto-scaling-group-name my-asg
    --min-size 2
    --max-size 6
    --desired-capacity 3
```

### To delete an ASG

The following example deletes an auto scaling group. The group must have a
desired capacity of 0 first, otherwise the call fails.

```bash
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg
```

### To force delete an ASG (with instances)

The following example deletes an auto scaling group and terminates its
instances in one call.

```bash
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg --force-delete
```

### To suspend all scaling processes

The following example suspends every scaling process, which is useful before
making a manual change that would otherwise be reverted by a scaling policy.

```bash
aws autoscaling suspend-processes --auto-scaling-group-name my-asg
```

### To resume suspended processes

The following example resumes scaling processes suspended earlier.

```bash
aws autoscaling resume-processes --auto-scaling-group-name my-asg
```

### To view scaling activity history

The following example lists the scaling activities the group has performed,
newest first.

```bash
aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg
```

### To list instances in an ASG

The following example lists the instances currently in the group.

```bash
aws autoscaling describe-auto-scaling-groups
    --auto-scaling-group-name my-asg
    --query "AutoScalingGroups[0].Instances"
```

## Legacy Launch Configurations

Launch configurations are deprecated. They are documented here only so you can
inspect an existing group that still uses one. New groups must use a launch
template.

- Accounts created on or after 1 January 2023 cannot use new EC2 instance types
  in a launch configuration.
- Accounts created on or after 1 June 2023 cannot create launch configurations
  in the console.
- Accounts created on or after 1 October 2024 cannot create launch
  configurations at all, including through the CLI.

### To list existing launch configurations

The following example lists launch configurations still present in the account.

```bash
aws autoscaling describe-launch-configurations
```

### To find groups still using a launch configuration

The following example lists the auto scaling groups that still reference a
launch configuration instead of a launch template, so you know what needs
migrating.

```bash
aws autoscaling describe-auto-scaling-groups \
    --query "AutoScalingGroups[?LaunchConfigurationName!=`null`].[AutoScalingGroupName,LaunchConfigurationName]"
```

### To delete a launch configuration

The following example deletes a launch configuration once no group references
it.

```bash
aws autoscaling delete-launch-configuration --launch-configuration-name my-lc
```

## Scaling Policies

### To create a target tracking policy (CPU)

The following example creates a target tracking policy (CPU).

```bash
aws autoscaling put-scaling-policy
    --auto-scaling-group-name my-asg
    --policy-name scale-out
    --policy-type TargetTrackingScaling
    --target-tracking-configuration "PredefinedMetricSpecification={PredefinedMetricType=ASGAverageCPUUtilization},TargetValue=70.0"
```

### To create a step scaling policy

The following example creates a step scaling policy.

```bash
aws autoscaling put-scaling-policy
    --auto-scaling-group-name my-asg
    --policy-name scale-out
    --policy-type StepScaling
    --adjustment-type ChangeInCapacity
    --step-adjustments "Magnitude=1"
```

### To create a simple scaling policy

The following example creates a simple scaling policy.

```bash
aws autoscaling put-scaling-policy
    --auto-scaling-group-name my-asg
    --policy-name scale-out
    --policy-type SimpleScaling
    --adjustment-type ChangeInCapacity
    --scaling-adjustment 1
```

### To list scaling policies for an ASG

The following example lists scaling policies for an ASG.

```bash
aws autoscaling describe-policies --auto-scaling-group-name my-asg
```

### To delete a scaling policy

The following example deletes a scaling policy.

```bash
aws autoscaling delete-policy --auto-scaling-group-name my-asg --policy-name scale-out
```

## Scheduled Actions

### To schedule a scaling action

The following example schedules a scaling action.

```bash
aws autoscaling put-scheduled-update-group-action
    --auto-scaling-group-name my-asg
    --scheduled-action-name scale-up-morning
    --recurrence "0 8 * * *"
    --min-size 4
    --max-size 8
    --desired-capacity 5
```

### To list scheduled actions

The following example lists scheduled actions.

```bash
aws autoscaling describe-scheduled-actions --auto-scaling-group-name my-asg
```

### To delete a scheduled action

The following example deletes a scheduled action.

```bash
aws autoscaling delete-scheduled-action
    --auto-scaling-group-name my-asg
    --scheduled-action-name scale-up-morning
```

## Instance Refresh

### To start an instance refresh (rolling update)

The following example starts an instance refresh (rolling update).

```bash
aws autoscaling start-instance-refresh --auto-scaling-group-name my-asg
```

### To check instance refresh status

The following example checks instance refresh status.

```bash
aws autoscaling describe-instance-refreshes --auto-scaling-group-name my-asg
```
