# Policies

> Policies. Part of the [IAM](../) cheatsheet.

## To list all IAM policies

## To list only customer-managed policies

The following example lists only customer-managed policies.

```bash
aws iam list-policies --scope Local
```

## To list only AWS-managed policies

The following example lists only AWS-managed policies.

```bash
aws iam list-policies --scope AWS
```

## To get details for a specific policy

The following example gets details for a specific policy.

```bash
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy
```

## To create a customer-managed policy

The following example creates a customer-managed policy.

```bash
aws iam create-policy --policy-name my-policy --policy-document file://policy.json
```

## To delete a customer-managed policy

The following example deletes a customer-managed policy.

```bash
aws iam delete-policy --policy-arn arn:aws:iam::123456789012:policy/my-policy
```

## To get a specific policy version

The following example gets a specific policy version.

```bash
aws iam get-policy-version --policy-arn arn:... --version-id v1
```

## To list all versions of a policy

The following example lists all versions of a policy.

```bash
aws iam list-policy-versions --policy-arn arn:...
```

## To create a new policy version

The following example creates a new policy version.

```bash
aws iam create-policy-version
    --policy-arn arn:...
    --policy-document file://policy.json
    --set-as-default
```

## To delete a non-default policy version

The following example deletes a non-default policy version.

```bash
aws iam delete-policy-version --policy-arn arn:... --version-id v2
```
