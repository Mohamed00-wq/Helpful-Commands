# Repositories

> Repositories. Part of the [ECR](../) cheatsheet.

## To create a repository

The following example creates a repository with scan on push enabled, so every
pushed image is checked for vulnerabilities automatically.

```bash
aws ecr create-repository \
    --repository-name <repository> \
    --image-scanning-configuration scanOnPush=true
```

## To get a repository URI

The following example returns the registry URI you need for `docker push`.

```bash
aws ecr describe-repositories --repository-names <repository> \
    --query 'repositories[0].repositoryUri' --output text
```

## To list repositories

## To get the registry id

The following example returns the 12 digit account id the registry belongs to.

```bash
aws ecr describe-registry --query 'registryId' --output text
```

## To delete a repository

The following example deletes a repository. Without `--force` it fails if the
repository still holds images.

```bash
aws ecr delete-repository --repository-name <repository> --force
```
