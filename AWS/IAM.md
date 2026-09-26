# IAM (Identity and Access Management)

> Commands for users, roles, policies, groups, instance profiles, and credential reports.

## Users

### To list all IAM users

The following example lists all IAM users.

```bash
aws iam list-users
```

### To get details for a specific user

The following example gets details for a specific user.

```bash
aws iam get-user --user-name alice
```

### To create a new IAM user

The following example creates a new IAM user.

```bash
aws iam create-user --user-name bob
```

### To delete an IAM user

The following example deletes an IAM user.

```bash
aws iam delete-user --user-name bob
```

### To generate access keys for a user

The following example generates access keys for a user.

```bash
aws iam create-access-key --user-name bob
```

### To list access keys for a user

The following example lists access keys for a user.

```bash
aws iam list-access-keys --user-name bob
```

### To delete an access key

The following example deletes an access key.

```bash
aws iam delete-access-key --user-name bob --access-key-id AKIA...
```

### To deactivate an access key

The following example deactivates an access key.

```bash
aws iam update-access-key --user-name bob --access-key-id AKIA... --status Inactive
```

### To create a console login profile

The following example creates a console login profile.

```bash
aws iam create-login-profile --user-name bob --password P@ssw0rd!
```

### To update console password

The following example updates console password.

```bash
aws iam update-login-profile --user-name bob --password P@ssw0rd!
```

### To remove console access

The following example removes console access.

```bash
aws iam delete-login-profile --user-name bob
```

## Roles

### To list all IAM roles

The following example lists all IAM roles.

```bash
aws iam list-roles
```

### To get details for a specific role

The following example gets details for a specific role.

```bash
aws iam get-role --role-name my-role
```

### To create a role with a trust policy

The following example creates a role with a trust policy.

```bash
aws iam create-role --role-name my-role --assume-role-policy-document file://trust.json
```

### To delete a role

The following example deletes a role.

```bash
aws iam delete-role --role-name my-role
```

### To update role session duration

The following example updates role session duration.

```bash
aws iam update-role --role-name my-role --max-session-duration 3600
```

### To get an inline policy from a role

The following example gets an inline policy from a role.

```bash
aws iam get-role-policy --role-name my-role --policy-name my-policy
```

### To list inline policies on a role

The following example lists inline policies on a role.

```bash
aws iam list-role-policies --role-name my-role
```

### To list managed policies attached to a role

The following example lists managed policies attached to a role.

```bash
aws iam list-attached-role-policies --role-name my-role
```

### To attach a managed policy to a role

The following example attaches a managed policy to a role.

```bash
aws iam attach-role-policy
    --role-name my-role
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

### To detach a managed policy from a role

The following example detaches a managed policy from a role.

```bash
aws iam detach-role-policy
    --role-name my-role
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

### To attach an inline policy to a role

The following example attaches an inline policy to a role.

```bash
aws iam put-role-policy
    --role-name my-role
    --policy-name inline-policy
    --policy-document file://policy.json
```

### To remove an inline policy from a role

The following example removes an inline policy from a role.

```bash
aws iam delete-role-policy --role-name my-role --policy-name inline-policy
```

## Policies

### To list all IAM policies

### To list only customer-managed policies

The following example lists only customer-managed policies.

```bash
aws iam list-policies --scope Local
```

### To list only AWS-managed policies

The following example lists only AWS-managed policies.

```bash
aws iam list-policies --scope AWS
```

### To get details for a specific policy

The following example gets details for a specific policy.

```bash
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy
```

### To create a customer-managed policy

The following example creates a customer-managed policy.

```bash
aws iam create-policy --policy-name my-policy --policy-document file://policy.json
```

### To delete a customer-managed policy

The following example deletes a customer-managed policy.

```bash
aws iam delete-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy
```

### To get a specific policy version

The following example gets a specific policy version.

```bash
aws iam get-policy-version --policy-arn arn:... --version-id v1
```

### To list all versions of a policy

The following example lists all versions of a policy.

```bash
aws iam list-policy-versions --policy-arn arn:...
```

### To create a new policy version

The following example creates a new policy version.

```bash
aws iam create-policy-version
    --policy-arn arn:...
    --policy-document file://policy.json
    --set-as-default
```

### To delete a non-default policy version

The following example deletes a non-default policy version.

```bash
aws iam delete-policy-version --policy-arn arn:... --version-id v2
```

## User-Policy Attachments

### To attach a managed policy to a user

The following example attaches a managed policy to a user.

```bash
aws iam attach-user-policy --user-name alice --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

### To detach a managed policy from a user

The following example detaches a managed policy from a user.

```bash
aws iam detach-user-policy --user-name alice --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

### To list managed policies attached to a user

The following example lists managed policies attached to a user.

```bash
aws iam list-attached-user-policies --user-name alice
```

### To list inline policies attached to a user

The following example lists inline policies attached to a user.

```bash
aws iam list-user-policies --user-name alice
```

### To attach an inline policy to a user

The following example attaches an inline policy to a user.

```bash
aws iam put-user-policy
    --user-name alice
    --policy-name inline-policy
    --policy-document file://policy.json
```

### To remove an inline policy from a user

The following example removes an inline policy from a user.

```bash
aws iam delete-user-policy --user-name alice --policy-name inline-policy
```

### To get an inline policy from a user

The following example gets an inline policy from a user.

```bash
aws iam get-user-policy --user-name alice --policy-name inline-policy
```

## Groups

### To list all IAM groups

The following example lists all IAM groups.

```bash
aws iam list-groups
```

### To get details for a specific group

The following example gets details for a specific group.

```bash
aws iam get-group --group-name my-group
```

### To create a new group

The following example creates a new group.

```bash
aws iam create-group --group-name my-group
```

### To delete a group

The following example deletes a group.

```bash
aws iam delete-group --group-name my-group
```

### To add a user to a group

The following example adds a user to a group.

```bash
aws iam add-user-to-group --user-name alice --group-name my-group
```

### To remove a user from a group

The following example removes a user from a group.

```bash
aws iam remove-user-from-group --user-name alice --group-name my-group
```

### To list groups a user belongs to

The following example lists groups a user belongs to.

```bash
aws iam list-groups-for-user --user-name alice
```

### To list managed policies attached to a group

The following example lists managed policies attached to a group.

```bash
aws iam list-attached-group-policies --group-name my-group
```

### To attach a managed policy to a group

The following example attaches a managed policy to a group.

```bash
aws iam attach-group-policy
    --group-name my-group
    --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

### To detach a managed policy from a group

The following example detaches a managed policy from a group.

```bash
aws iam detach-group-policy
    --group-name my-group
    --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
```

### To list inline policies on a group

The following example lists inline policies on a group.

```bash
aws iam list-group-policies --group-name my-group
```

### To attach an inline policy to a group

The following example attaches an inline policy to a group.

```bash
aws iam put-group-policy
    --group-name my-group
    --policy-name inline-policy
    --policy-document file://policy.json
```

## STS (Security Token Service)

### To show currently authenticated identity

The following example shows currently authenticated identity.

```bash
aws sts get-caller-identity
```

### To get session token with MFA

The following example gets session token with MFA.

```bash
aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/alice --token-code 123456
```

### To assume an IAM role

The following example assumes an IAM role.

```bash
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name "session1"
```

### To assume a role with custom duration

The following example assumes a role with custom duration.

```bash
aws sts assume-role
    --role-arn arn:aws:iam::123456789012:role/my-role
    --role-session-name "session1"
    --duration-seconds 900
```

### To assume a role with external ID

The following example assumes a role with external ID.

```bash
aws sts assume-role
    --role-arn arn:aws:iam::123456789012:role/my-role
    --role-session-name "session1"
    --external-id ABCD1234
```

### To get federated user credentials

The following example gets federated user credentials.

```bash
aws sts get-federation-token --name alice --policy file://policy.json
```

### To decode an access denied error message

The following example decodes an access denied error message.

```bash
aws sts decode-authorization-message --encoded-message "..."
```

## MFA

### To list MFA devices for a user

The following example lists MFA devices for a user.

```bash
aws iam list-mfa-devices --user-name alice
```

### To enable an MFA device

The following example enables an MFA device.

```bash
aws iam enable-mfa-device
    --user-name alice
    --serial-number arn:aws:iam::123456789012:mfa/alice
    --authentication-code1 123456
    --authentication-code2 789012
```

### To deactivate an MFA device

The following example deactivates an MFA device.

```bash
aws iam deactivate-mfa-device --user-name alice --serial-number arn:aws:iam::123456789012:mfa/alice
```

### To resync an MFA device

The following example resyncs an MFA device.

```bash
aws iam resync-mfa-device
    --user-name alice
    --serial-number arn:aws:iam::123456789012:mfa/alice
    --authentication-code1 123456
    --authentication-code2 789012
```

## Instance Profiles

### To list instance profiles

The following example lists instance profiles.

```bash
aws iam list-instance-profiles
```

### To get details for a specific profile

The following example gets details for a specific profile.

```bash
aws iam get-instance-profile --instance-profile-name my-profile
```

### To create an instance profile

The following example creates an instance profile.

```bash
aws iam create-instance-profile --instance-profile-name my-profile
```

### To delete an instance profile

The following example deletes an instance profile.

```bash
aws iam delete-instance-profile --instance-profile-name my-profile
```

### To attach a role to an instance profile

The following example attaches a role to an instance profile.

```bash
aws iam add-role-to-instance-profile --instance-profile-name my-profile --role-name my-role
```

### To remove a role from an instance profile

The following example removes a role from an instance profile.

```bash
aws iam remove-role-from-instance-profile --instance-profile-name my-profile --role-name my-role
```

## Credential Reports

### To generate a credential report

The following example generates a credential report.

```bash
aws iam generate-credential-report
```

### To download the credential report (CSV)

The following example downloads the credential report (CSV).

```bash
aws iam get-credential-report
```
