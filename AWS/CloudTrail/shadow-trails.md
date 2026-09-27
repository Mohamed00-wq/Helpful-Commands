# Shadow Trails

> Shadow Trails. Part of the [CloudTrail](../CloudTrail.md) cheatsheet.

Shadow trails are copies of a member account's trail created automatically for
a partner integration, such as a SIEM. The AWS CLI has no call to create one;
you list and read them like any other trail.

## To list shadow trails

The following example includes shadow trails, which `describe-trails` leaves
out by default.

```bash
aws cloudtrail describe-trails --include-shadow-trails
```

## To read a shadow trail's configuration

The following example returns one shadow trail by name, and its ARN is the
resource ID that `add-tags` and `list-tags` expect.

```bash
aws cloudtrail describe-trails \
    --trail-name-list <trail> \
    --include-shadow-trails
```

## To read a shadow trail's selectors

The following example returns the data events a partner is collecting, which
is the usual reason to look at a shadow trail.

```bash
aws cloudtrail get-event-selectors --trail-name <trail>
```
