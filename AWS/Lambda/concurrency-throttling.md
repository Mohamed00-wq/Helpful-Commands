# Concurrency & Throttling

> Concurrency & Throttling. Part of the [Lambda](../Lambda.md) cheatsheet.

## To get reserved concurrency

The following example gets reserved concurrency.

```bash
aws lambda get-function-concurrency --function-name my-function
```

## To set reserved concurrency

The following example sets reserved concurrency.

```bash
aws lambda put-function-concurrency --function-name my-function --reserved-concurrent-executions 100
```

## To remove reserved concurrency

The following example removes reserved concurrency.

```bash
aws lambda delete-function-concurrency --function-name my-function
```

## To set provisioned concurrency

The following example sets provisioned concurrency.

```bash
aws lambda put-provisioned-concurrency-config
    --function-name my-function
    --qualifier PROD
    --provisioned-concurrent-executions 10
```

## To get provisioned concurrency

The following example gets provisioned concurrency.

```bash
aws lambda get-provisioned-concurrency-config --function-name my-function --qualifier PROD
```

## To remove provisioned concurrency

The following example removes provisioned concurrency.

```bash
aws lambda delete-provisioned-concurrency-config --function-name my-function --qualifier PROD
```
