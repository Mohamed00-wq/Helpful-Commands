# Validate & Estimate

> Validate & Estimate. Part of the [CloudFormation](../) cheatsheet.

## To validate a template

The following example validates a template.

```bash
aws cloudformation validate-template --template-body file://template.json
```

## To estimate template cost

The following example estimates template cost.

```bash
aws cloudformation estimate-template-cost --template-body file://template.json
```

## To get template summary

The following example gets template summary.

```bash
aws cloudformation get-template-summary --template-body file://template.json
```
