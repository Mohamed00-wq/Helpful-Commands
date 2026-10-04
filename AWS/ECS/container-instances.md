# Container Instances (EC2 Launch Type)

> Container Instances (EC2 Launch Type). Part of the [ECS](../) cheatsheet.

## To list container instances

The following example lists container instances.

```bash
aws ecs list-container-instances --cluster my-cluster
```

## To get instance details

The following example gets instance details.

```bash
aws ecs describe-container-instances --cluster my-cluster --container-instances instance-arn
```

## To drain an instance

The following example drains an instance.

```bash
aws ecs drain-container-instance --cluster my-cluster --container-instances instance-arn
```

## To register an EC2 instance

The following example registers an EC2 instance.

```bash
aws ecs register-container-instance
    --cluster my-cluster
    --instance-identity-document file://identity.json
```
