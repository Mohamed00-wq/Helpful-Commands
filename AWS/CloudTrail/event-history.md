# Event History

> Event History. Part of the [CloudTrail](../) cheatsheet.

Event history is a searchable copy of management events kept in the region for
the configured retention window, 90 days by default. It never contains data
events, and it is what `lookup-events` reads.

## To find recent management events

The following example returns the last few hours of management events in the
current region.

```bash
aws cloudtrail lookup-events \
    --start-time "2026-09-26T00:00:00Z" \
    --end-time "2026-09-26T12:00:00Z" \
    --max-items 50
```

## To find one API call by name

The following example returns every `DeleteTrail` call, and you may only set
one attribute key per lookup.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=EventName,AttributeValue=DeleteTrail \
    --start-time "2026-09-26T00:00:00Z"
```

## To find calls made by a user

The following example returns every call made by one IAM user.

```bash
aws cloudtrail lookup-events \
    --lookup-attributes AttributeKey=Username,AttributeValue=<username> \
    --start-time "2026-09-26T00:00:00Z"
```

## To find insight and digest events

The following example returns the anomaly and digest events rather than the
raw API calls, which is a cheap way to see what CloudTrail flagged.

```bash
aws cloudtrail lookup-events --event-category insight --max-items 10
```

## To page through event history

The following example returns the next page, because `lookup-events` returns
at most 50 events per call and stops there.

```bash
aws cloudtrail lookup-events \
    --start-time "2026-09-26T00:00:00Z" \
    --starting-token <next-token>
```
