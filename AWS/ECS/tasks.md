# Tasks

> Tasks. Part of the [ECS](../ECS.md) cheatsheet.

## To list running tasks

The following example lists running tasks.

```bash
aws ecs list-tasks --cluster my-cluster
```

## To list tasks for a service

The following example lists tasks for a service.

```bash
aws ecs list-tasks --cluster my-cluster --service-name my-service
```

## To get task details

The following example gets task details.

```bash
aws ecs describe-tasks --cluster my-cluster --tasks task-arn-1 task-arn-2
```

## To run a one-off task

The following example runs a one-off task.

```bash
aws ecs run-task
    --cluster my-cluster
    --task-definition my-task:1
    --launch-type FARGATE
    --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx]}"
```

## To stop a running task

The following example stops a running task.

```bash
aws ecs stop-task --cluster my-cluster --task task-arn-1 --reason "manual stop"
```

```bash
# Run one-off task
aws ecs run-task \
    --cluster my-cluster \
    --task-definition my-task:1 \
    --launch-type FARGATE \
    --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx]}"

# Stop task
aws ecs stop-task --cluster my-cluster --task task-arn-1
```
