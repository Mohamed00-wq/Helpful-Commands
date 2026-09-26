# ECS (Elastic Container Service)

> Commands for clusters, services, tasks, task definitions, and Fargate.

## Clusters

### To list all clusters

The following example lists all clusters.

```bash
aws ecs list-clusters
```

### To get cluster details

The following example gets cluster details.

```bash
aws ecs describe-clusters --clusters my-cluster
```

### To create a cluster

The following example creates a cluster.

```bash
aws ecs create-cluster --cluster-name my-cluster
```

### To create with capacity providers

The following example creates with capacity providers.

```bash
aws ecs create-cluster --cluster-name my-cluster --capacity-providers FARGATE FARGATE_SPOT
```

### To delete a cluster

The following example deletes a cluster.

```bash
aws ecs delete-cluster --cluster my-cluster
```

## Task Definitions

### To list all task definitions

The following example lists all task definitions.

```bash
aws ecs list-task-definitions
```

### To get task definition details

The following example gets task definition details.

```bash
aws ecs describe-task-definition --task-definition my-task:1
```

### To register a task definition

The following example registers a task definition.

```bash
aws ecs register-task-definition --cli-input-json file://task-def.json
```

### To deregister a revision

The following example deregisters a revision.

```bash
aws ecs deregister-task-definition --task-definition my-task:1
```

### To list task definition families

The following example lists task definition families.

```bash
aws ecs list-task-definition-families
```

## Services

### To list services in a cluster

The following example lists services in a cluster.

```bash
aws ecs list-services --cluster my-cluster
```

### To get service details

The following example gets service details.

```bash
aws ecs describe-services --cluster my-cluster --services my-service
```

### To create a Fargate service

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

### To scale a service

The following example scales a service.

```bash
aws ecs update-service --cluster my-cluster --service my-service --desired-count 4
```

### To update task definition

The following example updates task definition.

```bash
aws ecs update-service --cluster my-cluster --service my-service --task-definition my-task:2
```

### To delete a service

The following example deletes a service.

```bash
aws ecs delete-service --cluster my-cluster --service my-service
```

### To force delete a service

The following example forces delete a service.

```bash
aws ecs delete-service --cluster my-cluster --service my-service --force
```

### To check deployment status

The following example checks deployment status.

```bash
aws ecs describe-services
    --cluster my-cluster
    --services my-service
    --query "services[0].deployments"
```

## Tasks

### To list running tasks

The following example lists running tasks.

```bash
aws ecs list-tasks --cluster my-cluster
```

### To list tasks for a service

The following example lists tasks for a service.

```bash
aws ecs list-tasks --cluster my-cluster --service-name my-service
```

### To get task details

The following example gets task details.

```bash
aws ecs describe-tasks --cluster my-cluster --tasks task-arn-1 task-arn-2
```

### To run a one-off task

The following example runs a one-off task.

```bash
aws ecs run-task
    --cluster my-cluster
    --task-definition my-task:1
    --launch-type FARGATE
    --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx]}"
```

### To stop a running task

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

## Container Instances (EC2 Launch Type)

### To list container instances

The following example lists container instances.

```bash
aws ecs list-container-instances --cluster my-cluster
```

### To get instance details

The following example gets instance details.

```bash
aws ecs describe-container-instances --cluster my-cluster --container-instances instance-arn
```

### To drain an instance

The following example drains an instance.

```bash
aws ecs drain-container-instance --cluster my-cluster --container-instances instance-arn
```

### To register an EC2 instance

The following example registers an EC2 instance.

```bash
aws ecs register-container-instance
    --cluster my-cluster
    --instance-identity-document file://identity.json
```

## Services Discovery (Cloud Map)

### To list namespaces

The following example lists namespaces.

```bash
aws servicediscovery list-namespaces
```

### To create a private namespace

The following example creates a private namespace.

```bash
aws servicediscovery create-private-dns-namespace --name my.local --vpc vpc-xxx
```

### To register a service

The following example registers a service.

```bash
aws servicediscovery create-service --name my-svc --namespace-id ns-xxx
```

### To discover instances

The following example discovers instances.

```bash
aws servicediscovery discover-instances --namespace-name my.local --service-name my-svc
```
