# Tags

> Tags. Part of the [Lambda](../Lambda.md) cheatsheet.

## To list tags

The following example lists tags.

```bash
aws lambda list-tags --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function
```

## To add tags

The following example adds tags.

```bash
aws lambda tag-resource
    --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function
    --tags env=prod,team=backend
```

## To remove tags

The following example removes tags.

```bash
aws lambda untag-resource
    --resource arn:aws:lambda:us-east-1:ACCOUNT:function:my-function
    --tag-keys env
```
