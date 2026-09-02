# 🐳 ECS (Elastic Container Service)

> Advanced ECS CLI commands for clusters, services, tasks, and container instances — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Clusters

| Command | Description |
|---|---|
| `aws ecs list-clusters` | List all clusters |
| `aws ecs describe-clusters --clusters my-cluster` | Get cluster details |
| `aws ecs create-cluster --cluster-name my-cluster` | Create a cluster |
| `aws ecs create-cluster --cluster-name my-cluster --capacity-providers FARGATE FARGATE_SPOT` | Create with capacity providers |
| `aws ecs delete-cluster --cluster my-cluster` | Delete a cluster |

```bash
# Create FARGATE cluster
aws ecs create-cluster \
  --cluster-name my-cluster \
  --capacity-providers FARGATE FARGATE_SPOT

# Delete
aws ecs delete-cluster --cluster my-cluster
```

---

## Task Definitions

| Command | Description |
|---|---|
| `aws ecs list-task-definitions` | List all task definitions |
| `aws ecs describe-task-definition --task-definition my-task:1` | Get task definition details |
| `aws ecs register-task-definition --cli-input-json file://task-def.json` | Register a task definition |
| `aws ecs deregister-task-definition --task-definition my-task:1` | Deregister a revision |
| `aws ecs list-task-definition-families` | List task definition families |

```bash
# task-def.json example (Fargate):
# {
#   "family": "my-task",
#   "networkMode": "awsvpc",
#   "requiresCompatibilities": ["FARGATE"],
#   "cpu": "256",
#   "memory": "512",
#   "containerDefinitions": [
#     {
#       "name": "app",
#       "image": "nginx:latest",
#       "portMappings": [{"containerPort": 80, "protocol": "tcp"}]
#     }
#   ]
# }

aws ecs register-task-definition --cli-input-json file://task-def.json
```

---

## Services

| Command | Description |
|---|---|
| `aws ecs list-services --cluster my-cluster` | List services in a cluster |
| `aws ecs describe-services --cluster my-cluster --services my-service` | Get service details |
| `aws ecs create-service --cluster my-cluster --service-name my-service --task-definition my-task:1 --desired-count 2 --launch-type FARGATE --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx],AssignPublicIp=ENABLED}"` | Create a Fargate service |
| `aws ecs update-service --cluster my-cluster --service my-service --desired-count 4` | Scale a service |
| `aws ecs update-service --cluster my-cluster --service my-service --task-definition my-task:2` | Update task definition |
| `aws ecs delete-service --cluster my-cluster --service my-service` | Delete a service |
| `aws ecs delete-service --cluster my-cluster --service my-service --force` | Force delete a service |
| `aws ecs describe-services --cluster my-cluster --services my-service --query "services[0].deployments"` | Check deployment status |

```bash
# Create Fargate service
aws ecs create-service \
  --cluster my-cluster \
  --service-name my-service \
  --task-definition my-task:1 \
  --desired-count 2 \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx],AssignPublicIp=ENABLED}"

# Scale
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --desired-count 4

# Update task
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --task-definition my-task:2
```

---

## Tasks

| Command | Description |
|---|---|
| `aws ecs list-tasks --cluster my-cluster` | List running tasks |
| `aws ecs list-tasks --cluster my-cluster --service-name my-service` | List tasks for a service |
| `aws ecs describe-tasks --cluster my-cluster --tasks task-arn-1 task-arn-2` | Get task details |
| `aws ecs run-task --cluster my-cluster --task-definition my-task:1 --launch-type FARGATE --network-configuration "awsvpcConfiguration={Subnets=[subnet-xxx],SecurityGroups=[sg-xxx]}"` | Run a one-off task |
| `aws ecs stop-task --cluster my-cluster --task task-arn-1 --reason "manual stop"` | Stop a running task |

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

---

## Container Instances (EC2 Launch Type)

| Command | Description |
|---|---|
| `aws ecs list-container-instances --cluster my-cluster` | List container instances |
| `aws ecs describe-container-instances --cluster my-cluster --container-instances instance-arn` | Get instance details |
| `aws ecs drain-container-instance --cluster my-cluster --container-instances instance-arn` | Drain an instance |
| `aws ecs register-container-instance --cluster my-cluster --instance-identity-document file://identity.json` | Register an EC2 instance |

```bash
# Drain instance (for maintenance)
aws ecs drain-container-instance \
  --cluster my-cluster \
  --container-instances instance-arn
```

---

## Services Discovery (Cloud Map)

| Command | Description |
|---|---|
| `aws servicediscovery list-namespaces` | List namespaces |
| `aws servicediscovery create-private-dns-namespace --name my.local --vpc vpc-xxx` | Create a private namespace |
| `aws servicediscovery create-service --name my-svc --namespace-id ns-xxx` | Register a service |
| `aws servicediscovery discover-instances --namespace-name my.local --service-name my-svc` | Discover instances |

```bash
aws servicediscovery create-private-dns-namespace \
  --name my.local \
  --vpc vpc-xxx
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
