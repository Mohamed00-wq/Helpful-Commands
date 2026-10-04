# CloudWatch Logs

> CloudWatch Logs. Part of the [CloudTrail](../) cheatsheet.

A trail can send the same events to a log group as well as to S3, which lets
`filter-log-events` search them without touching the bucket.

## To deliver logs to a log group

The following example adds CloudWatch Logs delivery to a trail, where the role
needs `logs:CreateLogStream` and `logs:PutLogEvents` on the log group.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --cloud-watch-logs-log-group-arn \
        arn:aws:logs:<region>:<account-id>:log-group:<log-group>:* \
    --cloud-watch-logs-role-arn arn:aws:iam::<account-id>:role/<role>
```

## To stop the CloudWatch Logs delivery

The following example removes the log group and role from the trail, which
leaves S3 delivery untouched.

```bash
aws cloudtrail update-trail \
    --name <trail> \
    --cloud-watch-logs-log-group-arn "" \
    --cloud-watch-logs-role-arn ""
```

## To search the delivered events

The following example filters the trail's events in the log group, which is the
fast path for a single investigation.

```bash
aws logs filter-log-events \
    --log-group-name <log-group> \
    --filter-pattern '{ $.eventName = "DeleteTrail" }' \
    --start-time 1790294400000
```
