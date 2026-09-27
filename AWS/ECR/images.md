# Images

> Images. Part of the [ECR](../ECR.md) cheatsheet.

## To push an image

The following example builds and pushes an image, tagging it with the commit
SHA so every build is traceable.

```bash
docker build -t <repository>:<commit-sha> .
docker tag <repository>:<commit-sha> <account-id>.dkr.ecr.<region>.amazonaws.com/<repository>:<commit-sha>
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/<repository>:<commit-sha>
```

## To list image tags

The following example lists the tags on a repository.

```bash
aws ecr describe-images --repository-name <repository> \
    --query 'imageIds[].imageTag' --output text
```

## To list the five most recently pushed images

The following example sorts by push time and takes the last five, which is the
quickest way to see what is actually deployed.

```bash
aws ecr describe-images --repository-name <repository> \
    --query 'sort_by(imageDetails,& imagePushedAt)[-5:].[imageTags[0],imagePushedAt]' \
    --output table
```

## To get the digest of an image

The following example returns the immutable digest of a tag, which is what you
should deploy rather than the tag.

```bash
aws ecr describe-images --repository-name <repository> \
    --image-ids imageTag=<tag> \
    --query 'imageDetails[0].imageDigest' --output text
```

## To delete an image

The following example deletes one image by tag. The tag is deregistered but the
underlying layers can remain until the next garbage collection.

```bash
aws ecr batch-delete-image --repository-name <repository> --image-ids imageTag=<tag>
```

## To delete every image except the ones in use

The following example keeps the most recent ten images and deregisters the rest.

```bash
aws ecr describe-images --repository-name <repository> \
    --query 'imageIds[0:10]' --output json
```
