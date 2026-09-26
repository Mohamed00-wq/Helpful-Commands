# EKS (Elastic Kubernetes Service)

> Commands for clusters, node groups, Fargate profiles, addons, and kubeconfig access.

## Cluster Operations

### To list all EKS clusters

The following example lists all EKS clusters.

```bash
aws eks list-clusters
```

### To get cluster details

The following example gets cluster details.

```bash
aws eks describe-cluster --name my-cluster
```

### To create a cluster

The following example creates a cluster.

```bash
aws eks create-cluster
    --name my-cluster
    --role-arn arn:aws:iam::ACCOUNT:role/EKSClusterRole
    --resources-vpc-config subnetIds=subnet-xxx,subnet-yyy
```

### To delete a cluster

The following example deletes a cluster.

```bash
aws eks delete-cluster --name my-cluster
```

### To enable cluster logging

The following example enables cluster logging.

```bash
aws eks update-cluster-config
    --name my-cluster
    --logging '{"clusterLogging":[{"types":["api","audit"],"enabled":true}]}'
```

### To get OIDC issuer URL

The following example gets OIDC issuer URL.

```bash
aws eks describe-cluster --name my-cluster --query "cluster.identity.oidc.issuer"
```

## Node Groups

### To list node groups

The following example lists node groups.

```bash
aws eks list-nodegroups --cluster-name my-cluster
```

### To get node group details

The following example gets node group details.

```bash
aws eks describe-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes
```

### To create a managed node group

The following example creates a managed node group.

```bash
aws eks create-nodegroup
    --cluster-name my-cluster
    --nodegroup-name my-nodes
    --node-role arn:aws:iam::ACCOUNT:role/NodeRole
    --instance-types t3.medium
    --scaling-config minSize=2,maxSize=5,desiredSize=3
    --subnets subnet-xxx subnet-yyy
```

### To scale a node group

The following example scales a node group.

```bash
aws eks update-nodegroup-config
    --cluster-name my-cluster
    --nodegroup-name my-nodes
    --scaling-config minSize=1,maxSize=10,desiredSize=5
```

### To delete a node group

The following example deletes a node group.

```bash
aws eks delete-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes
```

### To update node group AMI

The following example updates node group AMI.

```bash
aws eks update-nodegroup-version --cluster-name my-cluster --nodegroup-name my-nodes
```

## Fargate Profiles

### To list Fargate profiles

The following example lists Fargate profiles.

```bash
aws eks list-fargate-profiles --cluster-name my-cluster
```

### To get profile details

The following example gets profile details.

```bash
aws eks describe-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile
```

### To create a Fargate profile

The following example creates a Fargate profile.

```bash
aws eks create-fargate-profile
    --cluster-name my-cluster
    --fargate-profile-name my-profile
    --pod-execution-role-arn arn:aws:iam::ACCOUNT:role/FargateRole
    --subnets subnet-xxx
    --selectors namespace=default
```

### To delete a Fargate profile

The following example deletes a Fargate profile.

```bash
aws eks delete-fargate-profile --cluster-name my-cluster --fargate-profile-name my-profile
```

## Addons

### To list installed addons

The following example lists installed addons.

```bash
aws eks list-addons --cluster-name my-cluster
```

### To get addon details

The following example gets addon details.

```bash
aws eks describe-addon --cluster-name my-cluster --addon-name vpc-cni
```

### To install an addon

The following example installs an addon.

```bash
aws eks create-addon --cluster-name my-cluster --addon-name vpc-cni --resolve-conflicts OVERWRITE
```

### To update an addon

The following example updates an addon.

```bash
aws eks update-addon --cluster-name my-cluster --addon-name vpc-cni --addon-version v1.15.1
```

### To delete an addon

The following example deletes an addon.

```bash
aws eks delete-addon --cluster-name my-cluster --addon-name vpc-cni
```

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

## Access & Authentication

### To update kubeconfig for the cluster

The following example updates kubeconfig for the cluster.

```bash
aws eks update-kubeconfig --name my-cluster
```

### To update kubeconfig for specific region

The following example updates kubeconfig for specific region.

```bash
aws eks update-kubeconfig --name my-cluster --region us-west-2
```

### To list access entries

The following example lists access entries.

```bash
aws eks list-access-entries --cluster-name my-cluster
```

### To add access entry

The following example adds access entry.

```bash
aws eks create-access-entry
    --cluster-name my-cluster
    --principal-arn arn:aws:iam::ACCOUNT:user/alice
```

### To remove access entry

The following example removes access entry.

```bash
aws eks delete-access-entry
    --cluster-name my-cluster
    --principal-arn arn:aws:iam::ACCOUNT:user/alice
```

### To attach access policy

The following example attaches access policy.

```bash
aws eks associate-access-policy
    --cluster-name my-cluster
    --principal-arn arn:aws:iam::ACCOUNT:user/alice
    --access-policy-arn arn:aws:eks::aws:cluster-access-policy/AmazonEKSClusterAdminPolicy
```

## Identity Providers (OIDC)

### To get OIDC details

The following example gets OIDC details.

```bash
aws eks describe-identity-provider-config
    --cluster-name my-cluster
    --identity-provider-config-name my-oidc
```

### To associate OIDC provider

The following example associates OIDC provider.

```bash
aws eks associate-identity-provider-config
    --cluster-name my-cluster
    --identity-provider-config '{"type":"oidc","name":"my-oidc","oidc":{"issuerUrl":"https://xxx","clientId":"xxx"}}'
```

### To disassociate OIDC

The following example disassociates OIDC.

```bash
aws eks disassociate-identity-provider-config
    --cluster-name my-cluster
    --identity-provider-config-name my-oidc
```
