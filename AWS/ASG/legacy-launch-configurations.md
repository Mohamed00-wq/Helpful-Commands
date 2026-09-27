# Legacy Launch Configurations

> Legacy Launch Configurations. Part of the [ASG](../ASG.md) cheatsheet.

Launch configurations are deprecated. They are documented here only so you can
inspect an existing group that still uses one. New groups must use a launch
template.

- Accounts created on or after 1 January 2023 cannot use new EC2 instance types
  in a launch configuration.
- Accounts created on or after 1 June 2023 cannot create launch configurations
  in the console.
- Accounts created on or after 1 October 2024 cannot create launch
  configurations at all, including through the CLI.

## To list existing launch configurations

The following example lists launch configurations still present in the account.

```bash
aws autoscaling describe-launch-configurations
```

## To find groups still using a launch configuration

The following example lists the auto scaling groups that still reference a
launch configuration instead of a launch template, so you know what needs
migrating.

```bash
aws autoscaling describe-auto-scaling-groups \
    --query "AutoScalingGroups[?LaunchConfigurationName!=`null`].[AutoScalingGroupName,LaunchConfigurationName]"
```

## To delete a launch configuration

The following example deletes a launch configuration once no group references
it.

```bash
aws autoscaling delete-launch-configuration --launch-configuration-name my-lc
```
