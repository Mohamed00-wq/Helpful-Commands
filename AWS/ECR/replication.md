# Replication

> Replication. Part of the [ECR](../) cheatsheet.

## To enable registry replication

The following example replicates images to a second region, which is how you
keep a backup or serve a multi-region deployment.

```bash
aws ecr put-registry-policy \
    --registry-id <account-id> \
    --policy-text file://replication-policy.json
```

`replication-policy.json`:

```json
{
  "rules": [
    {
      "rulePriority": 1,
      "description": "replicate to secondary region",
      "actions": ["REPLICATION"],
      "destinations": [
        {
          "region": "<region>",
          "registryId": "<account-id>"
        }
      ]
    }
  ]
}
```

## To list the replication rules

The following example returns the rules currently on the registry.

```bash
aws ecr describe-registry --query 'replicationConfiguration.rules' --output json
```
