# Snapshots

> Snapshots. Part of the [EBS](../) cheatsheet.

## To create a snapshot from a volume

The following example creates a snapshot from a volume.

```bash
aws ec2 create-snapshot --volume-id vol-xxxxxxxx --description "backup"
```

## To list your snapshots

The following example lists your snapshots.

```bash
aws ec2 describe-snapshots --owner-ids self
```

## To get details for a specific snapshot

The following example gets details for a specific snapshot.

```bash
aws ec2 describe-snapshots --snapshot-ids snap-xxxxxxxx
```

## To delete a snapshot

The following example deletes a snapshot.

```bash
aws ec2 delete-snapshot --snapshot-id snap-xxxxxxxx
```

## To copy a snapshot to another region

The following example copies a snapshot to another region.

```bash
aws ec2 copy-snapshot
    --source-region us-east-1
    --source-snapshot-id snap-xxxxxxxx
    --destination-region us-west-2
```

## To view snapshot sharing permissions

The following example views snapshot sharing permissions.

```bash
aws ec2 describe-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute createVolumePermission
```

## To share a snapshot with an account

The following example shares a snapshot with an account.

```bash
aws ec2 modify-snapshot-attribute
    --snapshot-id snap-xxxxxxxx
    --attribute createVolumePermission
    --operation-type add
    --user-ids 123456789012
```

## To make a snapshot public

The following example makes a snapshot public.

```bash
aws ec2 modify-snapshot-attribute
    --snapshot-id snap-xxxxxxxx
    --attribute createVolumePermission
    --operation-type add
    --group-names all
```

## To stop sharing a snapshot

The following example stops sharing a snapshot.

```bash
aws ec2 modify-snapshot-attribute
    --snapshot-id snap-xxxxxxxx
    --attribute createVolumePermission
    --operation-type remove
    --user-ids 123456789012
```

## To view product codes on a snapshot

The following example views product codes on a snapshot.

```bash
aws ec2 describe-snapshot-attribute --snapshot-id snap-xxxxxxxx --attribute productCodes
```
