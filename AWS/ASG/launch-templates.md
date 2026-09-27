# Launch Templates

> Launch Templates. Part of the [ASG](../ASG.md) cheatsheet.

## To create a launch template

The following example creates a launch template version that an auto scaling
group can launch instances from.

```bash
aws ec2 create-launch-template
    --launch-template-name my-lt
    --launch-template-data '{"ImageId":"ami-0123456789abcdef0","InstanceType":"t3.micro","KeyName":"my-key","SecurityGroupIds":["sg-0123456789abcdef0"]}'
```

## To create a launch template version

The following example publishes a new version of an existing launch template
without changing the default version.

```bash
aws ec2 create-launch-template-version
    --launch-template-name my-lt
    --launch-template-data '{"InstanceType":"t3.small"}'
    --version-description "upgraded instance type"
```

## To set a launch template version as the default

The following example marks a specific version as the default for new launches.

```bash
aws ec2 modify-launch-template
    --launch-template-name my-lt
    --set-default-version 2
```

## To list launch templates

The following example lists every launch template in the account.

```bash
aws ec2 describe-launch-templates
```

## To get launch template versions

The following example lists the versions of a launch template and which one is
the default.

```bash
aws ec2 describe-launch-template-versions --launch-template-name my-lt
```

## To get a launch template version

The following example retrieves one version, including the instance data that
an auto scaling group will use.

```bash
aws ec2 describe-launch-template-versions
    --launch-template-name my-lt
    --versions 1
```

## To delete a launch template

The following example deletes a launch template. It fails if the template is
still referenced by an auto scaling group.

```bash
aws ec2 delete-launch-template --launch-template-name my-lt
```
