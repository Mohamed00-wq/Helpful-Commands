# ECR (Elastic Container Registry)

> Commands for repositories, images, scanning, replication, and the Docker
> login that every CI pipeline needs.

## Authentication

### To log in to ECR

The following example authenticates Docker to the registry. The token is valid
for 12 hours, so re-run this step at the start of every pipeline job.

```bash
aws ecr get-login-password --region <region> \
  | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```

## Repositories

### To create a repository

The following example creates a repository with scan on push enabled, so every
pushed image is checked for vulnerabilities automatically.

```bash
aws ecr create-repository \
    --repository-name <repository> \
    --image-scanning-configuration scanOnPush=true
```

### To get a repository URI

The following example returns the registry URI you need for `docker push`.

```bash
aws ecr describe-repositories --repository-names <repository> \
    --query 'repositories[0].repositoryUri' --output text
```

### To list repositories

### To get the registry id

The following example returns the 12 digit account id the registry belongs to.

```bash
aws ecr describe-registry --query 'registryId' --output text
```

### To delete a repository

The following example deletes a repository. Without `--force` it fails if the
repository still holds images.

```bash
aws ecr delete-repository --repository-name <repository> --force
```

## Images

### To push an image

The following example builds and pushes an image, tagging it with the commit
SHA so every build is traceable.

```bash
docker build -t <repository>:<commit-sha> .
docker tag <repository>:<commit-sha> <account-id>.dkr.ecr.<region>.amazonaws.com/<repository>:<commit-sha>
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/<repository>:<commit-sha>
```

### To list image tags

The following example lists the tags on a repository.

```bash
aws ecr describe-images --repository-name <repository> \
    --query 'imageIds[].imageTag' --output text
```

### To list the five most recently pushed images

The following example sorts by push time and takes the last five, which is the
quickest way to see what is actually deployed.

```bash
aws ecr describe-images --repository-name <repository> \
    --query 'sort_by(imageDetails,& imagePushedAt)[-5:].[imageTags[0],imagePushedAt]' \
    --output table
```

### To get the digest of an image

The following example returns the immutable digest of a tag, which is what you
should deploy rather than the tag.

```bash
aws ecr describe-images --repository-name <repository> \
    --image-ids imageTag=<tag> \
    --query 'imageDetails[0].imageDigest' --output text
```

### To delete an image

The following example deletes one image by tag. The tag is deregistered but the
underlying layers can remain until the next garbage collection.

```bash
aws ecr batch-delete-image --repository-name <repository> --image-ids imageTag=<tag>
```

### To delete every image except the ones in use

The following example keeps the most recent ten images and deregisters the rest.

```bash
aws ecr describe-images --repository-name <repository> \
    --query 'imageIds[0:10]' --output json
```

## Scanning

### To start an on-demand scan

The following example scans an image even when scan on push is disabled.

```bash
aws ecr start-image-scan \
    --repository-name <repository> \
    --image-id imageTag=<tag>
```

### To check the scan findings

The following example lists findings. `HIGH` and `CRITICAL` are the severities
worth failing a build on.

```bash
aws ecr describe-image-scan-findings \
    --repository-name <repository> \
    --image-id imageTag=<tag> \
    --query 'imageScanFindings.findings[?severity==`HIGH` || severity==`CRITICAL`].[name,severity]' \
    --output table
```

## Replication

### To enable registry replication

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

### To list the replication rules

The following example returns the rules currently on the registry.

```bash
aws ecr describe-registry --query 'replicationConfiguration.rules' --output json
```

## Lifecycle

### To expire old images

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

## Workflows

### To deploy a container from ECR to ECS

The following sequence authenticates, pushes, then points the task definition
at the new image digest.

```bash
# 1. Authenticate
aws ecr get-login-password --region <region> \
  | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com

# 2. Build, tag, push
IMAGE_URI=$(aws ecr describe-repositories --repository-names <repository> \
  --query 'repositories[0].repositoryUri' --output text)
docker build -t "$IMAGE_URI:<tag>" .
docker push "$IMAGE_URI:<tag>"

# 3. Resolve the digest
DIGEST=$(aws ecr describe-images --repository-name <repository> \
  --image-ids imageTag=<tag> \
  --query 'imageDetails[0].imageDigest' --output text)

# 4. Update the task definition and deploy
aws ecs register-task-definition --cli-input-json file://task-definition.json
aws ecs update-service --cluster <cluster> --service <service> \
  --task-definition <task-definition-arn>
```
