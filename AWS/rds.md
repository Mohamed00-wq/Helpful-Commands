# 🗄️ RDS (Relational Database Service)

> Advanced RDS CLI commands for instances, clusters, snapshots, parameter groups, and read replicas — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## DB Instances

| Command | Description |
|---|---|
| `aws rds describe-db-instances` | List all RDS instances |
| `aws rds describe-db-instances --db-instance-identifier mydb` | Get details for a specific instance |
| `aws rds create-db-instance --db-instance-identifier mydb --db-instance-class db.t3.micro --engine mysql --master-username admin --master-user-password P@ssw0rd! --allocated-storage 20` | Create a MySQL instance |
| `aws rds create-db-instance --db-instance-identifier mydb --db-instance-class db.t3.micro --engine postgres --master-username admin --master-user-password P@ssw0rd! --allocated-storage 20 --db-name mydb` | Create a PostgreSQL instance |
| `aws rds delete-db-instance --db-instance-identifier mydb --skip-final-snapshot` | Delete without final snapshot |
| `aws rds delete-db-instance --db-instance-identifier mydb --final-db-snapshot-identifier mydb-final` | Delete with final snapshot |
| `aws rds stop-db-instance --db-instance-identifier mydb` | Stop an instance (non-Aurora) |
| `aws rds start-db-instance --db-instance-identifier mydb` | Start a stopped instance |
| `aws rds reboot-db-instance --db-instance-identifier mydb` | Reboot an instance |
| `aws rds describe-db-instance-automated-backups --db-instance-identifier mydb` | View automated backups |

```bash
# Create MySQL
aws rds create-db-instance \
  --db-instance-identifier mydb \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --master-username admin \
  --master-user-password P@ssw0rd! \
  --allocated-storage 20

# Create PostgreSQL
aws rds create-db-instance \
  --db-instance-identifier mydb \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password P@ssw0rd! \
  --allocated-storage 20 \
  --db-name mydb

# Delete
aws rds delete-db-instance \
  --db-instance-identifier mydb \
  --skip-final-snapshot
```

---

## Aurora Clusters

| Command | Description |
|---|---|
| `aws rds describe-db-clusters` | List all Aurora clusters |
| `aws rds describe-db-clusters --db-cluster-identifier my-cluster` | Get cluster details |
| `aws rds create-db-cluster --db-cluster-identifier my-cluster --engine aurora-mysql --master-username admin --master-user-password P@ssw0rd!` | Create Aurora MySQL cluster |
| `aws rds create-db-cluster --db-cluster-identifier my-cluster --engine aurora-postgresql --master-username admin --master-user-password P@ssw0rd!` | Create Aurora PostgreSQL cluster |
| `aws rds delete-db-cluster --db-cluster-identifier my-cluster --skip-final-snapshot` | Delete cluster without snapshot |
| `aws rds delete-db-cluster --db-cluster-identifier my-cluster --final-db-snapshot-identifier my-cluster-final` | Delete with final snapshot |
| `aws rds create-db-instance --db-cluster-identifier my-cluster --db-instance-identifier mydb-1 --db-instance-class db.r5.large --engine aurora-mysql` | Add instance to cluster |
| `aws rds describe-db-clusters --db-cluster-identifier my-cluster --query "DBClusters[0].Endpoint"` | Get cluster endpoint |

```bash
# Aurora MySQL cluster
aws rds create-db-cluster \
  --db-cluster-identifier my-cluster \
  --engine aurora-mysql \
  --master-username admin \
  --master-user-password P@ssw0rd!

# Add instance to cluster
aws rds create-db-instance \
  --db-cluster-identifier my-cluster \
  --db-instance-identifier mydb-1 \
  --db-instance-class db.r5.large \
  --engine aurora-mysql

# Delete
aws rds delete-db-cluster \
  --db-cluster-identifier my-cluster \
  --skip-final-snapshot
```

---

## Read Replicas

| Command | Description |
|---|---|
| `aws rds create-db-instance-read-replica --db-instance-identifier mydb-replica --source-db-instance-identifier mydb` | Create a read replica |
| `aws rds create-db-instance-read-replica --db-instance-identifier mydb-replica --source-db-instance-identifier mydb --source-region us-east-1 --region us-west-2` | Create cross-region read replica |
| `aws rds delete-db-instance --db-instance-identifier mydb-replica --skip-final-snapshot` | Delete a read replica |
| `aws rds promote-read-replica --db-instance-identifier mydb-replica` | Promote read replica to standalone |

```bash
# Same-region read replica
aws rds create-db-instance-read-replica \
  --db-instance-identifier mydb-replica \
  --source-db-instance-identifier mydb

# Cross-region read replica
aws rds create-db-instance-read-replica \
  --db-instance-identifier mydb-replica \
  --source-db-instance-identifier mydb \
  --source-region us-east-1 \
  --region us-west-2

# Promote
aws rds promote-read-replica --db-instance-identifier mydb-replica
```

---

## Snapshots

| Command | Description |
|---|---|
| `aws rds create-db-snapshot --db-snapshot-identifier my-snap --db-instance-identifier mydb` | Create a manual snapshot |
| `aws rds describe-db-snapshots --db-snapshot-identifier my-snap` | Get snapshot details |
| `aws rds describe-db-snapshots --db-instance-identifier mydb` | List snapshots for an instance |
| `aws rds delete-db-snapshot --db-snapshot-identifier my-snap` | Delete a manual snapshot |
| `aws rds copy-db-snapshot --source-db-snapshot-identifier my-snap --target-db-snapshot-identifier my-snap-copy --source-region us-east-1 --region us-west-2` | Copy snapshot to another region |
| `aws rds restore-db-instance-from-db-snapshot --db-instance-identifier mydb-restored --db-snapshot-identifier my-snap` | Restore from snapshot |
| `aws rds restore-db-instance-to-point-in-time --db-instance-identifier mydb-restored --source-db-instance-identifier mydb --restore-time "2026-01-15T12:00:00Z"` | Restore to point in time |
| `aws rds describe-db-snapshot-attributes --db-snapshot-identifier my-snap` | View snapshot sharing |
| `aws rds modify-db-snapshot-attribute --db-snapshot-identifier my-snap --attribute-name restore --values-to-add all` | Share snapshot publicly |

```bash
# Create snapshot
aws rds create-db-snapshot \
  --db-snapshot-identifier my-snap \
  --db-instance-identifier mydb

# Copy to another region
aws rds copy-db-snapshot \
  --source-db-snapshot-identifier my-snap \
  --target-db-snapshot-identifier my-snap-copy \
  --source-region us-east-1 \
  --region us-west-2

# Restore
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier mydb-restored \
  --db-snapshot-identifier my-snap
```

---

## Parameter Groups

| Command | Description |
|---|---|
| `aws rds describe-db-parameter-groups` | List parameter groups |
| `aws rds describe-db-parameters --db-parameter-group-name my-pg` | List parameters in a group |
| `aws rds create-db-parameter-group --db-parameter-group-name my-pg --db-parameter-group-family mysql8.0 --description "My parameter group"` | Create a parameter group |
| `aws rds delete-db-parameter-group --db-parameter-group-name my-pg` | Delete a parameter group |
| `aws rds modify-db-parameter-group --db-parameter-group-name my-pg --parameters "ParameterName=max_connections,ParameterValue=200,ApplyMethod=pending-reboot"` | Modify a parameter |
| `aws rds reset-db-parameter-group --db-parameter-group-name my-pg --parameters "ParameterName=max_connections,ApplyMethod=pending-reboot"` | Reset a parameter to default |

```bash
# Create parameter group
aws rds create-db-parameter-group \
  --db-parameter-group-name my-pg \
  --db-parameter-group-family mysql8.0 \
  --description "My parameter group"

# Modify parameter
aws rds modify-db-parameter-group \
  --db-parameter-group-name my-pg \
  --parameters "ParameterName=max_connections,ParameterValue=200,ApplyMethod=pending-reboot"
```

---

## Option Groups

| Command | Description |
|---|---|
| `aws rds describe-option-groups` | List option groups |
| `aws rds create-option-group --option-group-name my-og --engine-name mysql --major-engine-version 8.0 --description "My option group"` | Create an option group |
| `aws rds delete-option-group --option-group-name my-og` | Delete an option group |
| `aws rds add-option-to-option-group --option-group-name my-og --option-name MARIADB_AUDIT_PLUGIN --apply-immediately` | Add an option |
| `aws rds remove-option-from-option-group --option-group-name my-og --option-name MARIADB_AUDIT_PLUGIN --apply-immediately` | Remove an option |

```bash
aws rds create-option-group \
  --option-group-name my-og \
  --engine-name mysql \
  --major-engine-version 8.0 \
  --description "My option group"
```

---

## DB Subnet Groups

| Command | Description |
|---|---|
| `aws rds describe-db-subnet-groups` | List DB subnet groups |
| `aws rds create-db-subnet-group --db-subnet-group-name my-sg --subnet-ids "subnet-xxxxxxxx subnet-yyyyyyyy" --description "My DB subnet group"` | Create a subnet group |
| `aws rds delete-db-subnet-group --db-subnet-group-name my-sg` | Delete a subnet group |

```bash
aws rds create-db-subnet-group \
  --db-subnet-group-name my-sg \
  --subnet-ids "subnet-xxxxxxxx subnet-yyyyyyyy" \
  --description "My DB subnet group"
```

---

## Events & Notifications

| Command | Description |
|---|---|
| `aws rds describe-events --source-type db-instance --start-time 2026-01-15T00:00:00Z` | List RDS events |
| `aws rds describe-event-subscriptions` | List event subscriptions |
| `aws rds create-event-subscription --subscription-name my-sub --sns-topic-arn arn:aws:sns:us-east-1:123456789012:my-topic --source-type db-instance` | Create event subscription |
| `aws rds delete-event-subscription --subscription-name my-sub` | Delete event subscription |

```bash
# Events
aws rds describe-events --source-type db-instance \
  --start-time 2026-01-15T00:00:00Z

# Event subscription
aws rds create-event-subscription \
  --subscription-name my-sub \
  --sns-topic-arn arn:aws:sns:us-east-1:123456789012:my-topic \
  --source-type db-instance
```

---

## Global Database (Aurora)

| Command | Description |
|---|---|
| `aws rds describe-global-clusters` | List Aurora Global Databases |
| `aws rds create-global-cluster --global-cluster-identifier my-global --engine aurora-mysql` | Create a global cluster |
| `aws rds add-source-cluster-to-global-cluster --global-cluster-identifier my-global --source-db-cluster-arn arn:aws:rds:us-east-1:ACCOUNT:cluster:my-cluster` | Add primary cluster |
| `aws rds delete-global-cluster --global-cluster-identifier my-global` | Delete global cluster |

```bash
aws rds create-global-cluster \
  --global-cluster-identifier my-global \
  --engine aurora-mysql
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
