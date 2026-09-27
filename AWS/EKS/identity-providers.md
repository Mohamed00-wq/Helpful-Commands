# Identity Providers (OIDC)

> Identity Providers (OIDC). Part of the [EKS](../EKS.md) cheatsheet.

## To get OIDC details

The following example gets OIDC details.

```bash
aws eks describe-identity-provider-config
    --cluster-name my-cluster
    --identity-provider-config-name my-oidc
```

## To associate OIDC provider

The following example associates OIDC provider.

```bash
aws eks associate-identity-provider-config
    --cluster-name my-cluster
    --identity-provider-config '{"type":"oidc","name":"my-oidc","oidc":{"issuerUrl":"https://xxx","clientId":"xxx"}}'
```

## To disassociate OIDC

The following example disassociates OIDC.

```bash
aws eks disassociate-identity-provider-config
    --cluster-name my-cluster
    --identity-provider-config-name my-oidc
```
