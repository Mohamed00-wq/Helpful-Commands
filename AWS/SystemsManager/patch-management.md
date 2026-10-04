# Patch Management

> Patch Management. Part of the [SystemsManager](../) cheatsheet.

## To list patch groups

The following example lists patch groups.

```bash
aws ssm describe-patch-groups
```

## To list patch baselines

The following example lists patch baselines.

```bash
aws ssm describe-patch-baselines
```

## To create a patch baseline

The following example creates a patch baseline.

```bash
aws ssm create-patch-baseline --name my-baseline --operating-system AMAZON_LINUX_2
```

## To set default baseline

The following example sets default baseline.

```bash
aws ssm register-default-patch-baseline --baseline-id pb-xxx
```

## To scan for patches

The following example scans for patches.

```bash
aws ssm scan-patches --instance-ids i-xxx
```

## To list patches

The following example lists patches.

```bash
aws ssm describe-patches --filters "Key=CLASSIFICATION,Values=Security"
```

## To install patches

The following example installs patches.

```bash
aws ssm install-patches --instance-ids i-xxx --baseline-id pb-xxx --operation RebootIfNeeded
```

## To list patch installations

The following example lists patch installations.

```bash
aws ssm describe-patch-installations
```
