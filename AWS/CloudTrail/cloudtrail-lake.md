# CloudTrail Lake

> CloudTrail Lake. Part of the [CloudTrail](../) cheatsheet.

An event data store is the queryable, managed copy of a trail. It costs per
day of retention rather than per gigabyte stored, and the console runs SQL
against it.

## To create an event data store

The following example creates a data store that keeps seven days of
multi-region events.

```bash
aws cloudtrail create-event-data-store \
    --name <data-store> \
    --retention-period 7 \
    --multi-region-enabled \
    --termination-protection-enabled \
    --tags-list Key=Environment,Value=prod
```

## To check the status of a data store

The following example returns the data store's ARN and ingestion status,
because a store takes a few minutes to start ingesting.

```bash
aws cloudtrail get-event-data-store \
    --event-data-store arn:aws:cloudtrail:<region>:<account-id>:eventdatastore/<data-store>
```

## To list data stores

The following example returns every data store in the account or organization.

```bash
aws cloudtrail list-event-data-stores --max-results 10
```

## To send a partner's events into a data store

The following example creates a channel, which is how a partner integration
feeds CloudTrail Lake.

```bash
aws cloudtrail create-channel \
    --name <channel> \
    --source "aws-service/<service>" \
    --destinations file://channel-destinations.json
```

`channel-destinations.json`:

```json
[
  {
    "Type": "EVENT_DATA_STORE",
    "Location": "arn:aws:cloudtrail:<region>:<account-id>:eventdatastore/<data-store>"
  }
]
```

## To check a channel's ingestion status

The following example returns where a channel's events are going and whether
they are arriving.

```bash
aws cloudtrail get-channel --channel <channel>
```

## To delete a data store

The following example removes a data store and everything retained in it, so
unlock termination protection first or the call is rejected.

```bash
aws cloudtrail delete-event-data-store \
    --event-data-store arn:aws:cloudtrail:<region>:<account-id>:eventdatastore/<data-store>
```
