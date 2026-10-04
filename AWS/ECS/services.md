# Services

> Services. Part of the [ECS](../) cheatsheet.

## To list services in a cluster

The following example lists services in a cluster.

```bash
aws ecs list-services --cluster my-cluster
```

## To get service details

The following example gets service details.

```bash
aws ecs describe-services --cluster my-cluster --services my-service
```

## To create a Fargate service

The following example creates a Fargate service.

```bash
aws ecs create-service
    --cluster my-cluster
    --service-name my-service
    --task-definition my-task:1
    --desired-count 2
    --launch-type FARGATE
    --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx],AssignPublicIp=ENABLED}"
```

## To scale a service

The following example scales a service.

```bash
aws ecs update-service --cluster my-cluster --service my-service --desired-count 4
```

## To update task definition

The following example updates task definition.

```bash
aws ecs update-service --cluster my-cluster --service my-service --task-definition my-task:2
```

## To delete a service

The following example deletes a service.

```bash
aws ecs delete-service --cluster my-cluster --service my-service
```

## To force delete a service

The following example forces delete a service.

```bash
aws ecs delete-service --cluster my-cluster --service my-service --force
```

## To check deployment status

The following example checks deployment status.

```bash
aws ecs describe-services
    --cluster my-cluster
    --services my-service
    --query "services[0].deployments"
```
