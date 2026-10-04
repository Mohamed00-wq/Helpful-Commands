# Log Streams & Events

> Log Streams & Events. Part of the [CloudWatch](../) cheatsheet.

## To list log streams

The following example lists log streams.

```bash
aws logs describe-log-streams --log-group-name /my/app
```

## To get log events

The following example gets log events.

```bash
aws logs describe-log-events --log-group-name /my/app --log-stream-name my-stream
```

## To get events with filter

The following example gets events with filter.

```bash
aws logs describe-log-events
    --log-group-name /my/app
    --log-stream-name my-stream
    --start-time 1705276800000
    --filter-pattern "ERROR"
```

## To search across all streams

The following example searches across all streams.

```bash
aws logs filter-log-events
    --log-group-name /my/app
    --filter-pattern "ERROR"
    --start-time 1705276800000
```

## To put log events

The following example puts log events.

```bash
aws logs put-log-events
    --log-group-name /my/app
    --log-stream-name my-stream
    --log-events file://events.json
```
