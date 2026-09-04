# 🔐 IAM (Identity and Access Management)

> Essential IAM CLI commands for users, roles, policies, groups, MFA, STS, and more — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Users

| Command | Description |
|---|---|
| `aws iam list-users` | List all IAM users |
| `aws iam get-user --user-name alice` | Get details for a specific user |
| `aws iam create-user --user-name bob` | Create a new IAM user |
| `aws iam delete-user --user-name bob` | Delete an IAM user |
| `aws iam create-access-key --user-name bob` | Generate access keys for a user |
| `aws iam list-access-keys --user-name bob` | List access keys for a user |
| `aws iam delete-access-key --user-name bob --access-key-id AKIA...` | Delete an access key |
| `aws iam update-access-key --user-name bob --access-key-id AKIA... --status Inactive` | Deactivate an access key |
| `aws iam create-login-profile --user-name bob --password P@ssw0rd!` | Create a console login profile |
| `aws iam update-login-profile --user-name bob --password P@ssw0rd!` | Update console password |
| `aws iam delete-login-profile --user-name bob` | Remove console access |

```bash
aws iam list-users
aws iam get-user --user-name alice
aws iam create-user --user-name bob
aws iam delete-user --user-name bob
aws iam create-access-key --user-name bob
aws iam list-access-keys --user-name bob
aws iam delete-access-key --user-name bob --access-key-id AKIA...
aws iam update-access-key --user-name bob --access-key-id AKIA... --status Inactive
```

---

## Roles

| Command | Description |
|---|---|
| `aws iam list-roles` | List all IAM roles |
| `aws iam get-role --role-name my-role` | Get details for a specific role |
| `aws iam create-role --role-name my-role --assume-role-policy-document file://trust.json` | Create a role with a trust policy |
| `aws iam delete-role --role-name my-role` | Delete a role |
| `aws iam update-role --role-name my-role --max-session-duration 3600` | Update role session duration |
| `aws iam get-role-policy --role-name my-role --policy-name my-policy` | Get an inline policy from a role |
| `aws iam list-role-policies --role-name my-role` | List inline policies on a role |
| `aws iam list-attached-role-policies --role-name my-role` | List managed policies attached to a role |
| `aws iam attach-role-policy --role-name my-role --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess` | Attach a managed policy to a role |
| `aws iam detach-role-policy --role-name my-role --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess` | Detach a managed policy from a role |
| `aws iam put-role-policy --role-name my-role --policy-name inline-policy --policy-document file://policy.json` | Attach an inline policy to a role |
| `aws iam delete-role-policy --role-name my-role --policy-name inline-policy` | Remove an inline policy from a role |

```bash
aws iam list-roles
aws iam create-role \
  --role-name my-role \
  --assume-role-policy-document file://trust.json
aws iam delete-role --role-name my-role
aws iam attach-role-policy \
  --role-name my-role \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
aws iam list-attached-role-policies --role-name my-role
aws iam put-role-policy \
  --role-name my-role \
  --policy-name inline-policy \
  --policy-document file://policy.json
```

---

## Policies

| Command | Description |
|---|---|
| `aws iam list-policies` | List all IAM policies |
| `aws iam list-policies --scope Local` | List only customer-managed policies |
| `aws iam list-policies --scope AWS` | List only AWS-managed policies |
| `aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy` | Get details for a specific policy |
| `aws iam create-policy --policy-name my-policy --policy-document file://policy.json` | Create a customer-managed policy |
| `aws iam delete-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy` | Delete a customer-managed policy |
| `aws iam get-policy-version --policy-arn arn:... --version-id v1` | Get a specific policy version |
| `aws iam list-policy-versions --policy-arn arn:...` | List all versions of a policy |
| `aws iam create-policy-version --policy-arn arn:... --policy-document file://policy.json --set-as-default` | Create a new policy version |
| `aws iam delete-policy-version --policy-arn arn:... --version-id v2` | Delete a non-default policy version |

```bash
aws iam list-policies --scope Local
aws iam create-policy \
  --policy-name my-policy \
  --policy-document file://policy.json
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy
aws iam list-policy-versions --policy-arn arn:...
aws iam create-policy-version \
  --policy-arn arn:... \
  --policy-document file://policy.json \
  --set-as-default
```

---

## User-Policy Attachments

| Command | Description |
|---|---|
| `aws iam attach-user-policy --user-name alice --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess` | Attach a managed policy to a user |
| `aws iam detach-user-policy --user-name alice --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess` | Detach a managed policy from a user |
| `aws iam list-attached-user-policies --user-name alice` | List managed policies attached to a user |
| `aws iam list-user-policies --user-name alice` | List inline policies attached to a user |
| `aws iam put-user-policy --user-name alice --policy-name inline-policy --policy-document file://policy.json` | Attach an inline policy to a user |
| `aws iam delete-user-policy --user-name alice --policy-name inline-policy` | Remove an inline policy from a user |
| `aws iam get-user-policy --user-name alice --policy-name inline-policy` | Get an inline policy from a user |

```bash
aws iam attach-user-policy \
  --user-name alice \
  --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
aws iam list-attached-user-policies --user-name alice
aws iam list-user-policies --user-name alice
aws iam put-user-policy \
  --user-name alice \
  --policy-name inline-policy \
  --policy-document file://policy.json
```

---

## Groups

| Command | Description |
|---|---|
| `aws iam list-groups` | List all IAM groups |
| `aws iam get-group --group-name my-group` | Get details for a specific group |
| `aws iam create-group --group-name my-group` | Create a new group |
| `aws iam delete-group --group-name my-group` | Delete a group |
| `aws iam add-user-to-group --user-name alice --group-name my-group` | Add a user to a group |
| `aws iam remove-user-from-group --user-name alice --group-name my-group` | Remove a user from a group |
| `aws iam list-groups-for-user --user-name alice` | List groups a user belongs to |
| `aws iam list-attached-group-policies --group-name my-group` | List managed policies attached to a group |
| `aws iam attach-group-policy --group-name my-group --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess` | Attach a managed policy to a group |
| `aws iam detach-group-policy --group-name my-group --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess` | Detach a managed policy from a group |
| `aws iam list-group-policies --group-name my-group` | List inline policies on a group |
| `aws iam put-group-policy --group-name my-group --policy-name inline-policy --policy-document file://policy.json` | Attach an inline policy to a group |

```bash
aws iam list-groups
aws iam create-group --group-name my-group
aws iam add-user-to-group --user-name alice --group-name my-group
aws iam list-groups-for-user --user-name alice
aws iam attach-group-policy \
  --group-name my-group \
  --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
aws iam list-attached-group-policies --group-name my-group
```

---

## STS (Security Token Service)

| Command | Description |
|---|---|
| `aws sts get-caller-identity` | Show currently authenticated identity |
| `aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/alice --token-code 123456` | Get session token with MFA |
| `aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name "session1"` | Assume an IAM role |
| `aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name "session1" --duration-seconds 900` | Assume a role with custom duration |
| `aws sts assume-role --role-arn arn:aws:iam::123456789012:role/my-role --role-session-name "session1" --external-id ABCD1234` | Assume a role with external ID |
| `aws sts get-federation-token --name alice --policy file://policy.json` | Get federated user credentials |
| `aws sts decode-authorization-message --encoded-message "..."` | Decode an access denied error message |

```bash
aws sts get-caller-identity
aws sts get-session-token \
  --serial-number arn:aws:iam::123456789012:mfa/alice \
  --token-code 123456
aws sts assume-role \
  --role-arn arn:aws:iam::123456789012:role/my-role \
  --role-session-name "session1"
aws sts assume-role \
  --role-arn arn:aws:iam::123456789012:role/my-role \
  --role-session-name "session1" \
  --external-id ABCD1234
aws sts decode-authorization-message --encoded-message "..."
```

---

## MFA

| Command | Description |
|---|---|
| `aws iam list-mfa-devices --user-name alice` | List MFA devices for a user |
| `aws iam enable-mfa-device --user-name alice --serial-number arn:aws:iam::123456789012:mfa/alice --authentication-code1 123456 --authentication-code2 789012` | Enable an MFA device |
| `aws iam deactivate-mfa-device --user-name alice --serial-number arn:aws:iam::123456789012:mfa/alice` | Deactivate an MFA device |
| `aws iam resync-mfa-device --user-name alice --serial-number arn:aws:iam::123456789012:mfa/alice --authentication-code1 123456 --authentication-code2 789012` | Resync an MFA device |

```bash
aws iam list-mfa-devices --user-name alice
aws iam enable-mfa-device \
  --user-name alice \
  --serial-number arn:aws:iam::123456789012:mfa/alice \
  --authentication-code1 123456 \
  --authentication-code2 789012
aws iam deactivate-mfa-device \
  --user-name alice \
  --serial-number arn:aws:iam::123456789012:mfa/alice
```

---

## Instance Profiles

| Command | Description |
|---|---|
| `aws iam list-instance-profiles` | List instance profiles |
| `aws iam get-instance-profile --instance-profile-name my-profile` | Get details for a specific profile |
| `aws iam create-instance-profile --instance-profile-name my-profile` | Create an instance profile |
| `aws iam delete-instance-profile --instance-profile-name my-profile` | Delete an instance profile |
| `aws iam add-role-to-instance-profile --instance-profile-name my-profile --role-name my-role` | Attach a role to an instance profile |
| `aws iam remove-role-from-instance-profile --instance-profile-name my-profile --role-name my-role` | Remove a role from an instance profile |

```bash
aws iam list-instance-profiles
aws iam create-instance-profile --instance-profile-name my-profile
aws iam add-role-to-instance-profile \
  --instance-profile-name my-profile --role-name my-role
aws iam remove-role-from-instance-profile \
  --instance-profile-name my-profile --role-name my-role
```

---

## Credential Reports

| Command | Description |
|---|---|
| `aws iam generate-credential-report` | Generate a credential report |
| `aws iam get-credential-report` | Download the credential report (CSV) |

```bash
aws iam generate-credential-report
aws iam get-credential-report
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
