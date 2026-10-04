# Fargate Profiles

> Fargate Profiles. Part of the [EKS](../) cheatsheet.

## To list Fargate profiles

The following example lists Fargate profiles.

```bash
aws eks list-fargate-profiles --cluster-name my-cluster
```

## To get profile details

The following example gets profile details.

```bash
aws eks describe-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile
```

## To create a Fargate profile

The following example creates a Fargate profile.

```bash
aws eks create-fargate-profile
    --cluster-name my-cluster
    --fargate-profile-name my-profile
    --pod-execution-role-arn arn:aws:iam::ACCOUNT:role/FargateRole
    --subnets subnet-xxx
    --selectors namespace=default
```

## To delete a Fargate profile

The following example deletes a Fargate profile.

```bash
aws eks delete-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile
```
