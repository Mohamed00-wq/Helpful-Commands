# Log Files

> Log Files. Part of the [CloudTrail](../CloudTrail.md) cheatsheet.

CloudTrail writes gzipped JSON Lines to S3, so every file holds many records,
one JSON object per line. `jq` reads that stream directly, and the files
arrive about fifteen minutes behind the events. When log file validation is
on, a `CloudTrail-Digest` folder sits alongside the dated folders and holds
the checksums.

## To list the log objects for one day

The following example lists the objects for a single day, because the
`AWSLogs` prefix is partitioned by account, region, and date.

```bash
aws s3 ls "s3://<bucket>/AWSLogs/<account-id>/CloudTrail/<region>/2026/09/26/" \
    --recursive --region <region>
```

## To download the log files

The following example fetches the whole trail prefix, which also pulls the
`CloudTrail-Digest` folder you need for the tamper check below.

```bash
aws s3 sync \
    "s3://<bucket>/AWSLogs/<account-id>/CloudTrail/<region>/" \
    ./cloudtrail-logs/ --region <region>
```

## To read the records in a log file

The following example prints one line per record without printing the binary
body, which keeps the output readable.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -c '{time: .eventTime, name: .eventName, who: .userIdentity.arn}'
```

## To count the calls made by each principal

The following example tallies calls per user or assumed role, which is how you
find the noisiest principal in an account.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -r '.userIdentity.arn // "unknown"' \
  | sort | uniq -c | sort -rn | head
```

## To list the failed calls

The following example counts errors by code, which is how a misconfigured
automation shows up in a trail.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -r 'select(.errorCode != null) | .errorCode' \
  | sort | uniq -c | sort -rn
```

## To find calls from one IP address

The following example returns every call made from a single address, which
covers failed sign-ins and brute force attempts.

```bash
zcat ./cloudtrail-logs/*.json.gz \
  | jq -c 'select(.sourceIPAddress == "<ip-address>")
          | {time: .eventTime, name: .eventName, error: .errorCode}'
```

## To check whether a file was tampered with

The following example verifies the gzipped files against the digest files
CloudTrail writes next to them, which is only possible when log file
validation is enabled.

```bash
zcat ./cloudtrail-logs/CloudTrail-Digest/*.json.gz \
  | jq -r '.files[] | select(.filePath | endswith(".json.gz"))
          | "\(.hashHex)  \(.filePath)"' \
  | sha256sum --check --ignore-missing
```

Run it from the directory the digest's `filePath` values are relative to, and
every listed file must report `OK`.
