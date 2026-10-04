# Log Groups

> Log Groups. Part of the [CloudWatch](../) cheatsheet.

## To list all log groups

## To filter log groups

The following example filters log groups.

```bash
aws logs describe-log-groups --log-group-name-prefix /aws/lambda
```

## To create a log group

The following example creates a log group.

```bash
aws logs create-log-group --log-group-name /my/app
```

## To delete a log group

The following example deletes a log group.

```bash
aws logs delete-log-group --log-group-name /my/app
```

## To set retention policy

The following example sets retention policy.

```bash
aws logs put-retention-policy --log-group-name /my/app --retention-in-days 30
```

## To remove retention (infinite)

The following example removes retention (infinite).

```bash
aws logs delete-retention-policy --log-group-name /my/app
```
