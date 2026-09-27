# Node Groups

> Node Groups. Part of the [EKS](../EKS.md) cheatsheet.

## To list node groups

The following example lists node groups.

```bash
aws eks list-nodegroups --cluster-name my-cluster
```

## To get node group details

The following example gets node group details.

```bash
aws eks describe-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes
```

## To create a managed node group

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

## To scale a node group

The following example scales a node group.

```bash
aws eks update-nodegroup-config
    --cluster-name my-cluster
    --nodegroup-name my-nodes
    --scaling-config minSize=1,maxSize=10,desiredSize=5
```

## To delete a node group

The following example deletes a node group.

```bash
aws eks delete-nodegroup --cluster-name my-cluster --nodegroup-name my-nodes
```

## To update node group AMI

The following example updates node group AMI.

```bash
aws eks update-nodegroup-version --cluster-name my-cluster --nodegroup-name my-nodes
```
