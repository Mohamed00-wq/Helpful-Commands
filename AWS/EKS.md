# ☸️ EKS (Elastic Kubernetes Service)

> Advanced EKS CLI commands for clusters, node groups, and Kubernetes integration — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Cluster Operations

| Command | Description |
|---|---|
| `aws eks list-clusters` | List all EKS clusters |
| `aws eks describe-cluster --name my-cluster` | Get cluster details |
| `aws eks create-cluster --name my-cluster --role-arn arn:aws:iam::ACCOUNT:role/EKSClusterRole --resources-vpc-config subnetIds=subnet-xxx,subnet-yyy` | Create a cluster |
| `aws eks delete-cluster --name my-cluster` | Delete a cluster |
| `aws eks update-cluster-config --name my-cluster --logging '{"clusterLogging":[{"types":["api","audit"],"enabled":true}]}'` | Enable cluster logging |
| `aws eks describe-cluster --name my-cluster --query "cluster.identity.oidc.issuer"` | Get OIDC issuer URL |

```bash
# Create cluster
aws eks create-cluster \
  --name my-cluster \
  --role-arn arn:aws:iam::ACCOUNT:role/EKSClusterRole \
  --resources-vpc-config subnetIds=subnet-xxx,subnet-yyy

# Enable logging
aws eks update-cluster-config \
  --name my-cluster \
  --logging '{"clusterLogging":[{"types":["api","audit"],"enabled":true}]}'
```

---

## Node Groups

| Command | Description |
|---|---|
| `aws eks list-nodegroups --cluster-name my-cluster` | List node groups |
| `aws eks describe-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes` | Get node group details |
| `aws eks create-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes --node-role arn:aws:iam::ACCOUNT:role/NodeRole --instance-types t3.medium --scaling-config minSize=2,maxSize=5,desiredSize=3 --subnets subnet-xxx subnet-yyy` | Create a managed node group |
| `aws eks update-nodegroup-config --cluster-name my-cluster --nodegroup-name my-nodes --scaling-config minSize=1,maxSize=10,desiredSize=5` | Scale a node group |
| `aws eks delete-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes` | Delete a node group |
| `aws eks update-nodegroup-version --cluster-name my-cluster --nodegroup-name my-nodes` | Update node group AMI |

```bash
# Create node group
aws eks create-nodegroup \
  --cluster-name my-cluster \
  --nodegroup-name my-nodes \
  --node-role arn:aws:iam::ACCOUNT:role/NodeRole \
  --instance-types t3.medium \
  --scaling-config minSize=2,maxSize=5,desiredSize=3 \
  --subnets subnet-xxx subnet-yyy

# Scale
aws eks update-nodegroup-config \
  --cluster-name my-cluster \
  --nodegroup-name my-nodes \
  --scaling-config minSize=1,maxSize=10,desiredSize=5
```

---

## Fargate Profiles

| Command | Description |
|---|---|
| `aws eks list-fargate-profiles --cluster-name my-cluster` | List Fargate profiles |
| `aws eks describe-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile` | Get profile details |
| `aws eks create-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile --pod-execution-role-arn arn:aws:iam::ACCOUNT:role/FargateRole --subnets subnet-xxx --selectors namespace=default` | Create a Fargate profile |
| `aws eks delete-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile` | Delete a Fargate profile |

```bash
aws eks create-fargate-profile \
  --cluster-name my-cluster \
  --fargate-profile-name my-profile \
  --pod-execution-role-arn arn:aws:iam::ACCOUNT:role/FargateRole \
  --subnets subnet-xxx \
  --selectors namespace=default
```

---

## Addons

| Command | Description |
|---|---|
| `aws eks list-addons --cluster-name my-cluster` | List installed addons |
| `aws eks describe-addon --cluster-name my-cluster --addon-name vpc-cni` | Get addon details |
| `aws eks create-addon --cluster-name my-cluster --addon-name vpc-cni --resolve-conflicts OVERWRITE` | Install an addon |
| `aws eks update-addon --cluster-name my-cluster --addon-name vpc-cni --addon-version v1.15.1` | Update an addon |
| `aws eks delete-addon --cluster-name my-cluster --addon-name vpc-cni` | Delete an addon |

```bash
# Install VPC CNI
aws eks create-addon \
  --cluster-name my-cluster \
  --addon-name vpc-cni \
  --resolve-conflicts OVERWRITE

# Install CoreDNS
aws eks create-addon \
  --cluster-name my-cluster \
  --addon-name coredns \
  --resolve-conflicts OVERWRITE
```

---

## Access & Authentication

| Command | Description |
|---|---|
| `aws eks update-kubeconfig --name my-cluster` | Update kubeconfig for the cluster |
| `aws eks update-kubeconfig --name my-cluster --region us-west-2` | Update kubeconfig for specific region |
| `aws eks list-access-entries --cluster-name my-cluster` | List access entries |
| `aws eks create-access-entry --cluster-name my-cluster --principal-arn arn:aws:iam::ACCOUNT:user/alice` | Add access entry |
| `aws eks delete-access-entry --cluster-name my-cluster --principal-arn arn:aws:iam::ACCOUNT:user/alice` | Remove access entry |
| `aws eks associate-access-policy --cluster-name my-cluster --principal-arn arn:aws:iam::ACCOUNT:user/alice --access-policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy` | Attach access policy |

```bash
# Update kubeconfig
aws eks update-kubeconfig --name my-cluster

# Add user access
aws eks create-access-entry \
  --cluster-name my-cluster \
  --principal-arn arn:aws:iam::ACCOUNT:user/alice

aws eks associate-access-policy \
  --cluster-name my-cluster \
  --principal-arn arn:aws:iam::ACCOUNT:user/alice \
  --access-policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy
```

---

## Identity Providers (OIDC)

| Command | Description |
|---|---|
| `aws eks describe-identity-provider-config --cluster-name my-cluster --identity-provider-config-name my-oidc` | Get OIDC details |
| `aws eks associate-identity-provider-config --cluster-name my-cluster --identity-provider-config '{"type":"oidc","name":"my-oidc","oidc":{"issuerUrl":"https://xxx","clientId":"xxx"}}'` | Associate OIDC provider |
| `aws eks disassociate-identity-provider-config --cluster-name my-cluster --identity-provider-config-name my-oidc` | Disassociate OIDC |

```bash
aws eks associate-identity-provider-config \
  --cluster-name my-cluster \
  --identity-provider-config '{"type":"oidc","name":"my-oidc","oidc":{"issuerUrl":"https://xxx","clientId":"xxx"}}'
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
