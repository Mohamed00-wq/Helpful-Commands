# State Manager

> State Manager. Part of the [SystemsManager](../SystemsManager.md) cheatsheet.

## To list associations

The following example lists associations.

```bash
aws ssm list-associations
```

## To create association

The following example creates association.

```bash
aws ssm create-association
    --name "AWS-RunShellScript"
    --targets "Key=tag:env,Values=prod"
    --parameters commands=["yum update
    -y"]
```

## To get association details

The following example gets association details.

```bash
aws ssm describe-association --association-id xxx
```

## To delete association

The following example deletes association.

```bash
aws ssm delete-association --association-id xxx
```
