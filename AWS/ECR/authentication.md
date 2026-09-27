# Authentication

> Authentication. Part of the [ECR](../ECR.md) cheatsheet.

## To log in to ECR

The following example authenticates Docker to the registry. The token is valid
for 12 hours, so re-run this step at the start of every pipeline job.

```bash
aws ecr get-login-password --region <region> \
  | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```
