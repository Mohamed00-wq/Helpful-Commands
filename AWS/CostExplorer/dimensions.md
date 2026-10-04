# Dimensions

> Dimensions. Part of the [CostExplorer](../) cheatsheet.

## To list the service names

The following example returns the service names you can filter and group on,
which is the quickest way to get the exact spelling of a value.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension SERVICE \
    --search-string 'Amazon'
```

## To list the usage types of a service

The following example returns the usage types Cost Explorer bills a service
under, and these strings are what a `USAGE_TYPE` filter takes.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension USAGE_TYPE \
    --search-string 'BoxUsage'
```

## To list the usage types of a Reserved Instance

The following example switches the context to reservations, which is what
returns the normalized usage types a Reserved Instance covers.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension USAGE_TYPE \
    --context RESERVATIONS \
    --search-string 'BoxUsage'
```

## To page through dimension values

The following example caps a page at 20 values, and the `NextPageToken` from the
response is what `--next-page-token` takes to reach the next page.

```bash
aws ce get-dimension-values \
    --time-period Start=2026-01-01,End=2026-02-01 \
    --dimension SERVICE \
    --max-results 20
```
