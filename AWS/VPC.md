# VPC (Virtual Private Cloud)

> Commands for subnets, routing, NAT gateways, peering, endpoints, and flow logs.

## VPC Operations

### To list all VPCs

### To get VPC details

The following example gets VPC details.

```bash
aws ec2 describe-vpcs --vpc-ids vpc-xxxxxxxx
```

### To create a VPC

The following example creates a VPC.

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

### To delete a VPC

The following example deletes a VPC.

```bash
aws ec2 delete-vpc --vpc-id vpc-xxxxxxxx
```

### To enable DNS support

The following example enables DNS support.

```bash
aws ec2 modify-vpc-attribute --vpc-id vpc-xxxxxxxx --enable-dns-support
```

### To enable DNS hostnames

The following example enables DNS hostnames.

```bash
aws ec2 modify-vpc-attribute --vpc-id vpc-xxxxxxxx --enable-dns-hostnames
```

### To check DNS support

The following example checks DNS support.

```bash
aws ec2 describe-vpc-attribute --vpc-id vpc-xxxxxxxx --attribute enableDnsSupport
```

## Subnets

### To list all subnets

### To get subnet details

The following example gets subnet details.

```bash
aws ec2 describe-subnets --subnet-ids subnet-xxxxxxxx
```

### To create a subnet

The following example creates a subnet.

```bash
aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone us-east-1a
```

### To delete a subnet

The following example deletes a subnet.

```bash
aws ec2 delete-subnet --subnet-id subnet-xxxxxxxx
```

### To make subnet public

The following example makes subnet public.

```bash
aws ec2 modify-subnet-attribute --subnet-id subnet-xxxxxxxx --map-public-ip-on-launch
```

### To make subnet private

The following example makes subnet private.

```bash
aws ec2 modify-subnet-attribute --subnet-id subnet-xxxxxxxx --no-map-public-ip-on-launch
```

### To associate route table

The following example associates route table.

```bash
aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```

## Internet Gateway

### To list IGWs

The following example lists IGWs.

```bash
aws ec2 describe-internet-gateways
```

### To create an IGW

The following example creates an IGW.

```bash
aws ec2 create-internet-gateway
```

### To attach IGW to VPC

The following example attaches IGW to VPC.

```bash
aws ec2 attach-internet-gateway --internet-gateway-id igw-xxxxxxxx --vpc-id vpc-xxxxxxxx
```

### To detach IGW

The following example detaches IGW.

```bash
aws ec2 detach-internet-gateway --internet-gateway-id igw-xxxxxxxx --vpc-id vpc-xxxxxxxx
```

### To delete IGW

The following example deletes IGW.

```bash
aws ec2 delete-internet-gateway --internet-gateway-id igw-xxxxxxxx
```

## NAT Gateway

### To list NAT gateways

### To create a NAT gateway

The following example creates a NAT gateway.

```bash
aws ec2 create-nat-gateway --subnet-id subnet-xxxxxxxx --allocation-id eipalloc-xxxxxxxx
```

### To delete a NAT gateway

The following example deletes a NAT gateway.

```bash
aws ec2 delete-nat-gateway --nat-gateway-id nat-xxxxxxxx
```

### To list NAT gateways for a VPC

The following example lists NAT gateways for a VPC.

```bash
aws ec2 describe-nat-gateways --filter "Name=vpc-id,Values=vpc-xxx"
```

```bash
# Allocate EIP first
aws ec2 allocate-address --domain vpc

# Create NAT gateway
aws ec2 create-nat-gateway \
    --subnet-id subnet-xxx \
    --allocation-id eipalloc-xxx
```

## Route Tables

### To list route tables

The following example lists route tables.

```bash
aws ec2 describe-route-tables
```

### To create a route table

The following example creates a route table.

```bash
aws ec2 create-route-table --vpc-id vpc-xxxxxxxx
```

### To add route to IGW

The following example adds route to IGW.

```bash
aws ec2 create-route
    --route-table-id rtb-xxxxxxxx
    --destination-cidr-block 0.0.0.0/0
    --gateway-id igw-xxxxxxxx
```

### To add route to NAT

The following example adds route to NAT.

```bash
aws ec2 create-route
    --route-table-id rtb-xxxxxxxx
    --destination-cidr-block 0.0.0.0/0
    --nat-gateway-id nat-xxxxxxxx
```

### To delete a route

The following example deletes a route.

```bash
aws ec2 delete-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0
```

### To associate with subnet

The following example associates with subnet.

```bash
aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```

### To disassociate route table

The following example disassociates route table.

```bash
aws ec2 disassociate-route-table --association-id rtbassoc-xxxxxxxx
```

## VPC Peering

### To list peering connections

The following example lists peering connections.

```bash
aws ec2 describe-vpc-peering-connections
```

### To create peering request

The following example creates peering request.

```bash
aws ec2 create-vpc-peering-connection --vpc-id vpc-xxx --peer-vpc-id vpc-yyy
```

### To accept peering request

The following example accepts peering request.

```bash
aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx
```

### To reject peering

The following example rejects peering.

```bash
aws ec2 reject-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx
```

### To delete peering

The following example deletes peering.

```bash
aws ec2 delete-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx
```

```bash
# Create peering
aws ec2 create-vpc-peering-connection \
    --vpc-id vpc-xxx --peer-vpc-id vpc-yyy

# Accept (from peer VPC account)
aws ec2 accept-vpc-peering-connection \
    --vpc-peering-connection-id pcx-xxx

# Add routes (on both sides)
aws ec2 create-route \
    --route-table-id rtb-xxx \
    --destination-cidr-block 10.1.0.0/16 \
    --vpc-peering-connection-id pcx-xxx
```

## VPC Endpoints (PrivateLink)

### To list all endpoints

The following example lists all endpoints.

```bash
aws ec2 describe-vpc-endpoints
```

### To create S3 gateway endpoint

The following example creates S3 gateway endpoint.

```bash
aws ec2 create-vpc-endpoint
    --vpc-id vpc-xxx
    --service-name com.amazonaws.us-east-1.s3
    --route-table-ids rtb-xxx
```

### To create DynamoDB endpoint

The following example creates DynamoDB endpoint.

```bash
aws ec2 create-vpc-endpoint
    --vpc-id vpc-xxx
    --service-name com.amazonaws.us-east-1.dynamodb
    --route-table-ids rtb-xxx
```

### To create interface endpoint

The following example creates interface endpoint.

```bash
aws ec2 create-vpc-endpoint
    --vpc-id vpc-xxx
    --service-name com.amazonaws.us-east-1.sqs
    --vpc-endpoint-type Interface
    --subnet-ids subnet-xxx
    --security-group-ids sg-xxx
```

### To delete an endpoint

The following example deletes an endpoint.

```bash
aws ec2 delete-vpc-endpoints --vpc-endpoint-ids vpce-xxxxxxxx
```

### To list available endpoint services

The following example lists available endpoint services.

```bash
aws ec2 describe-vpc-endpoint-services
```

## Network ACLs

### To list all NACLs

The following example lists all NACLs.

```bash
aws ec2 describe-network-acls
```

### To create a NACL

The following example creates a NACL.

```bash
aws ec2 create-network-acl --vpc-id vpc-xxx
```

### To add outbound rule

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

### To add inbound rule

The following example adds inbound rule.

```bash
aws ec2 create-network-acl-entry
    --network-acl-id acl-xxx
    --rule-number 100
    --protocol tcp
    --port-range From=80,To=80
    --rule-action allow
```

### To delete outbound rule

The following example deletes outbound rule.

```bash
aws ec2 delete-network-acl-entry --network-acl-id acl-xxx --rule-number 100 --egress
```

### To associate NACL with subnet

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

## Flow Logs

### To list flow logs

The following example lists flow logs.

```bash
aws ec2 describe-flow-logs
```

### To create flow log to CloudWatch

The following example creates flow log to CloudWatch.

```bash
aws ec2 create-flow-logs
    --resource-type VPC
    --resource-ids vpc-xxx
    --traffic-type ALL
    --log-destination-type cloud-watch-logs
    --log-group-name /vpc/flowlogs
    --deliver-logs-permission-arn arn:aws:iam::ACCOUNT:role/FlowLogRole
```

### To create flow log to S3

The following example creates flow log to S3.

```bash
aws ec2 create-flow-logs
    --resource-type VPC
    --resource-ids vpc-xxx
    --traffic-type REJECT
    --log-destination-type s3
    --log-destination arn:aws:s3:::my-bucket/flowlogs/
```

### To delete flow logs

The following example deletes flow logs.

```bash
aws ec2 delete-flow-logs --flow-log-ids fl-xxxxxxxx
```

## Egress-Only Internet Gateway (IPv6)

### To create egress-only IGW

The following example creates egress-only IGW.

```bash
aws ec2 create-egress-only-internet-gateway --vpc-id vpc-xxx
```

### To list egress-only IGWs

The following example lists egress-only IGWs.

```bash
aws ec2 describe-egress-only-internet-gateways
```

### To delete egress-only IGW

The following example deletes egress-only IGW.

```bash
aws ec2 delete-egress-only-internet-gateway --egress-only-internet-gateway-id eigw-xxx
```

## Workflows

### To build a public VPC from scratch

The following steps build a minimal public VPC with one public subnet, an
internet gateway, a route table, a security group, and one instance. Run them in
order and capture the id each step returns, because every later step needs it.

1. Create the VPC and capture its id.

   ```bash
   VPC_ID=$(aws ec2 create-vpc --cidr-block 10.0.0.0/16 \
       --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=lab-vpc}]' \
       --query 'Vpc.VpcId' --output text)
   echo "$VPC_ID"
   ```

2. Turn on DNS support and DNS hostnames. Instance names do not resolve without
   this, and interface endpoints need it.

   ```bash
   aws ec2 modify-vpc-attribute --vpc-id "$VPC_ID" --enable-dns-support
   aws ec2 modify-vpc-attribute --vpc-id "$VPC_ID" --enable-dns-hostnames
   ```

3. Create a public subnet and capture its id.

   ```bash
   SUBNET_ID=$(aws ec2 create-subnet \
       --vpc-id "$VPC_ID" \
       --cidr-block 10.0.1.0/24 \
       --availability-zone <region>a \
       --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=lab-public-subnet}]' \
       --query 'Subnet.SubnetId' --output text)
   echo "$SUBNET_ID"
   ```

4. Create an internet gateway and attach it to the VPC.

   ```bash
   IGW_ID=$(aws ec2 create-internet-gateway \
       --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=lab-igw}]' \
       --query 'InternetGateway.InternetGatewayId' --output text)
   aws ec2 attach-internet-gateway --vpc-id "$VPC_ID" --internet-gateway-id "$IGW_ID"
   ```

5. Create a route table, add a default route to the internet gateway, and
   associate it with the subnet. Without the route the subnet stays private
   even though it has an internet gateway.

   ```bash
   RTB_ID=$(aws ec2 create-route-table --vpc-id "$VPC_ID" \
       --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=lab-rt}]' \
       --query 'RouteTable.RouteTableId' --output text)
   aws ec2 create-route \
       --route-table-id "$RTB_ID" \
       --destination-cidr-block 0.0.0.0/0 \
       --gateway-id "$IGW_ID"
   aws ec2 associate-route-table --route-table-id "$RTB_ID" --subnet-id "$SUBNET_ID"
   ```

6. Create a security group and allow SSH inbound. Replace `0.0.0.0/0` with
   your own address before doing this anywhere real.

   ```bash
   SG_ID=$(aws ec2 create-security-group \
       --group-name lab-sg \
       --description "SG for lab" \
       --vpc-id "$VPC_ID" \
       --query 'GroupId' --output text)
   aws ec2 authorize-security-group-ingress \
       --group-id "$SG_ID" \
       --protocol tcp --port 22 --cidr 0.0.0.0/0
   ```

7. Launch an instance into the subnet. AWS CLI v2 attaches the key pair and
   security group for you when you specify the subnet.

   ```bash
   aws ec2 run-instances \
       --image-id <ami-id> \
       --instance-type t3.micro \
       --key-name <key-name> \
       --subnet-id "$SUBNET_ID" \
       --security-group-ids "$SG_ID" \
       --count 1
   ```

### To build a private subnet with outbound internet

Private subnets have no route to the internet gateway. They reach the internet
through a NAT gateway in a public subnet, which is the pattern for databases
and internal services that still need to pull packages.

1. Allocate an Elastic IP for the NAT gateway.

   ```bash
   ALLOCATION_ID=$(aws ec2 allocate-address --domain vpc \
       --query 'AllocationId' --output text)
   echo "$ALLOCATION_ID"
   ```

2. Create the NAT gateway in the public subnet and wait for it to become
   available. A NAT gateway is billed hourly from the moment it is created, so
   delete it when you are finished.

   ```bash
   NAT_ID=$(aws ec2 create-nat-gateway \
       --subnet-id "$SUBNET_ID" \
       --allocation-id "$ALLOCATION_ID" \
       --query 'NatGateway.NatGatewayId' --output text)
   aws ec2 wait nat-gateway-available --nat-gateway-ids "$NAT_ID"
   ```

3. Create a private route table and point its default route at the NAT gateway.

   ```bash
   PRIVATE_RTB_ID=$(aws ec2 create-route-table --vpc-id "$VPC_ID" \
       --query 'RouteTable.RouteTableId' --output text)
   aws ec2 create-route \
       --route-table-id "$PRIVATE_RTB_ID" \
       --destination-cidr-block 0.0.0.0/0 \
       --nat-gateway-id "$NAT_ID"
   aws ec2 associate-route-table \
       --route-table-id "$PRIVATE_RTB_ID" \
       --subnet-id <private-subnet-id>
   ```

### To tear down a VPC

The following order matters. AWS refuses to delete a VPC while it still
contains instances, subnets, route table associations, or gateways, so unwind
it from the inside out.

```bash
# Terminate instances, then wait for them to leave
aws ec2 terminate-instances --instance-ids <instance-ids>
aws ec2 wait instance-terminated --instance-ids <instance-ids>

# Release the Elastic IP before deleting the NAT gateway
aws ec2 release-address --allocation-id <allocation-id>
aws ec2 delete-nat-gateway --nat-gateway-id <nat-gateway-id>

# Detach and delete the internet gateway
aws ec2 detach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <igw-id>
aws ec2 delete-internet-gateway --internet-gateway-id <igw-id>

# Delete subnets
aws ec2 delete-subnet --subnet-id <subnet-id>

# Dissociate route tables, then delete them
aws ec2 disassociate-route-table --association-id <association-id>
aws ec2 delete-route-table --route-table-id <rtb-id>

# Finally the VPC
aws ec2 delete-vpc --vpc-id <vpc-id>
```
