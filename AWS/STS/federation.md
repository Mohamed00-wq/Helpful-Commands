# Federation

> Federation. Part of the [STS](../) cheatsheet.

## To get a federated token

The following example returns temporary credentials for a federated user,
typically used by the AWS CLI federation endpoint.

```bash
aws sts get-federation-token --name my-federated-user
```
