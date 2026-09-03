# 🏗️ VPC Lab Setup (CLI)

> Step-by-step guide to build a complete VPC from scratch using the AWS CLI — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## 1. Create the VPC

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=saa-lab-vpc}]'
```

> Grab the `VpcId` from the output — you'll need it in every step that follows.

---

## 2. Create a (public) subnet

```bash
aws ec2 create-subnet \
  --vpc-id vpc-xxxxxxxx \
  --cidr-block 10.0.1.0/24 \
  --availability-zone us-east-1a \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=saa-lab-public-subnet}]'
```

---

## 3. Create an Internet Gateway and attach it to the VPC

> This lets your instances reach the internet.

```bash
aws ec2 create-internet-gateway --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=saa-lab-igw}]'

aws ec2 attach-internet-gateway --vpc-id vpc-xxxxxxxx --internet-gateway-id igw-xxxxxxxx
```

---

## 4. Create a Route Table, add a route to the Internet Gateway, and associate it with the subnet

```bash
aws ec2 create-route-table --vpc-id vpc-xxxxxxxx --tag-specifications 'ResourceType=route-table,Tags=[{Key=Name,Value=saa-lab-rt}]'

aws ec2 create-route --route-table-id rtb-xxxxxxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxxxxxx

aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```

---

## 5. Create a Security Group for this VPC and add an inbound SSH rule

```bash
aws ec2 create-security-group \
  --group-name saa-lab-sg \
  --description "SG for SAA lab" \
  --vpc-id vpc-xxxxxxxx

aws ec2 authorize-security-group-ingress \
  --group-id sg-xxxxxxxx \
  --protocol tcp --port 22 --cidr 0.0.0.0/0
```

---

## 6. Finally, launch an instance inside the subnet

```bash
aws ec2 run-instances \
  --image-id ami-xxxxxxxx \
  --instance-type t2.micro \
  --key-name my-key \
  --subnet-id subnet-xxxxxxxx \
  --security-group-ids sg-xxxxxxxx \
  --count 1
```

---

## 📄 License

Feel free to use, modify, and share this guide.
