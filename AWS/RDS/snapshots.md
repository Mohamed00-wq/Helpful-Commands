# Snapshots

> Snapshots. Part of the [RDS](../RDS.md) cheatsheet.

## To create a manual snapshot

The following example creates a manual snapshot.

```bash
aws rds create-db-snapshot --db-snapshot-identifier my-snap --db-instance-identifier mydb
```

## To get snapshot details

The following example gets snapshot details.

```bash
aws rds describe-db-snapshots --db-snapshot-identifier my-snap
```

## To list snapshots for an instance

The following example lists snapshots for an instance.

```bash
aws rds describe-db-snapshots --db-instance-identifier mydb
```

## To delete a manual snapshot

The following example deletes a manual snapshot.

```bash
aws rds delete-db-snapshot --db-snapshot-identifier my-snap
```

## To copy snapshot to another region

The following example copies snapshot to another region.

```bash
aws rds copy-db-snapshot
    --source-db-snapshot-identifier my-snap
    --target-db-snapshot-identifier my-snap-copy
    --source-region us-east-1
    --region us-west-2
```

## To restore from snapshot

The following example restores from snapshot.

```bash
aws rds restore-db-instance-from-db-snapshot
    --db-instance-identifier mydb-restored
    --db-snapshot-identifier my-snap
```

## To restore to point in time

The following example restores to point in time.

```bash
aws rds restore-db-instance-to-point-in-time
    --db-instance-identifier mydb-restored
    --source-db-instance-identifier mydb
    --restore-time "2026-01-15T12:00:00Z"
```

## To view snapshot sharing

The following example views snapshot sharing.

```bash
aws rds describe-db-snapshot-attributes --db-snapshot-identifier my-snap
```

## To share snapshot publicly

The following example shares snapshot publicly.

```bash
aws rds modify-db-snapshot-attribute
    --db-snapshot-identifier my-snap
    --attribute-name restore
    --values-to-add all
```
