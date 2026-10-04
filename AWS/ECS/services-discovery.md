# Services Discovery (Cloud Map)

> Services Discovery (Cloud Map). Part of the [ECS](../) cheatsheet.

## To list namespaces

The following example lists namespaces.

```bash
aws servicediscovery list-namespaces
```

## To create a private namespace

The following example creates a private namespace.

```bash
aws servicediscovery create-private-dns-namespace --name my.local --vpc vpc-xxx
```

## To register a service

The following example registers a service.

```bash
aws servicediscovery create-service --name my-svc --namespace-id ns-xxx
```

## To discover instances

The following example discovers instances.

```bash
aws servicediscovery discover-instances --namespace-name my.local --service-name my-svc
```
