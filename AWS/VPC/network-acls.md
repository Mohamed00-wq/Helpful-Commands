# Network ACLs

> Network ACLs. Part of the [VPC](../VPC.md) cheatsheet.

## To list all NACLs

The following example lists all NACLs.

```bash
aws ec2 describe-network-acls
```

## To create a NACL

The following example creates a NACL.

```bash
aws ec2 create-network-acl --vpc-id vpc-xxx
```

## To add outbound rule

The following example adds outbound rule.

```bash
aws ec2 create-network-acl-entry
    --network-acl-id acl-xxx
    --rule-number 100
    --protocol tcp
    --port-range From=80,To=80
    --rule-action allow
    --egress
```

## To add inbound rule

The following example adds inbound rule.

```bash
aws ec2 create-network-acl-entry
    --network-acl-id acl-xxx
    --rule-number 100
    --protocol tcp
    --port-range From=80,To=80
    --rule-action allow
```

## To delete outbound rule

The following example deletes outbound rule.

```bash
aws ec2 delete-network-acl-entry --network-acl-id acl-xxx --rule-number 100 --egress
```

## To associate NACL with subnet

The following example associates NACL with subnet.

```bash
aws ec2 replace-network-acl-subnet --network-acl-id acl-xxx --subnet-id subnet-xxx
```

```bash
# Create NACL
aws ec2 create-network-acl --vpc-id vpc-xxx

# Allow HTTP inbound
aws ec2 create-network-acl-entry \
    --network-acl-id acl-xxx \
    --rule-number 100 \
    --protocol tcp \
    --port-range From=80,To=80 \
    --rule-action allow

# Allow all outbound
aws ec2 create-network-acl-entry \
    --network-acl-id acl-xxx \
    --rule-number 100 \
    --protocol -1 \
    --rule-action allow \
    --egress
```
