# Lifecycle

> Lifecycle. Part of the [ECR](../ECR.md) cheatsheet.

## To expire old images

The following example applies a lifecycle policy that keeps the last 50 images
and deletes anything untagged after 14 days.

```bash
aws ecr put-lifecycle-policy \
    --repository-name <repository> \
    --lifecycle-policy-text file://lifecycle.json
```

`lifecycle.json`:

```json
{
  "rules": [
    {
      "rulePriority": 1,
      "description": "keep last 50 images, expire untagged after 14 days",
      "selection": {
        "tagStatus": "any",
        "countType": "imageCountMoreThan",
        "countNumber": 50,
        "countUnit": "images"
      },
      "action": {"type": "expire"},
      "expiration": {"daysSincePushed": 14, "untaggedSincePushed": 14}
    }
  ]
}
```
