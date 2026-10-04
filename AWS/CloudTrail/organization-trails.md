# Organization Trails

> Organization Trails. Part of the [CloudTrail](../) cheatsheet.

An organization trail is created in the management account or in a delegated
administrator account and collects events from every member account.

## To create an organization trail

The following example creates an organization trail in the management account,
which is the only account that can create one.

```bash
aws cloudtrail create-trail \
    --name <trail> \
    --s3-bucket-name <bucket> \
    --is-organization-trail \
    --is-multi-region-trail
```

## To create an organization trail in a delegated admin

The following example creates the same trail from the delegated administrator
account, which is what you use when security owns a separate member account.

```bash
aws cloudtrail create-trail \
    --name <trail> \
    --s3-bucket-name <bucket> \
    --is-organization-trail \
    --is-multi-region-trail \
    --profile <profile>
```

## To read the organization trail configuration

The following example returns the trail as the delegated administrator sees
it, including the flag that marks it as organization wide.

```bash
aws cloudtrail describe-trails \
    --query 'trailList[].[Name,IsOrganizationTrail,IsMultiRegionTrail]' \
    --profile <profile>
```

## To tag an organization trail

The following example tags the trail once for the whole organization, because
member accounts cannot change the shared trail.

```bash
aws cloudtrail add-tags \
    --resource-id arn:aws:cloudtrail:<region>:<account-id>:trail/<trail> \
    --tags-list Key=Environment,Value=prod
```
