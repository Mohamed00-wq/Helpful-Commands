# 📈 Auto Scaling Groups (ASG)

> Essential ASG CLI commands for managing auto scaling groups, launch configurations, and scaling policies — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## Auto Scaling Groups

| Command | Description |
|---|---|
| `aws autoscaling describe-auto-scaling-groups` | List all ASGs |
| `aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name my-asg` | Get details for a specific ASG |
| `aws autoscaling create-auto-scaling-group --auto-scaling-group-name my-asg --launch-configuration-name my-lc --min-size 1 --max-size 4 --desired-capacity 2 --vpc-zone-identifier "subnet-xxxxxxxx"` | Create an ASG |
| `aws autoscaling update-auto-scaling-group --auto-scaling-group-name my-asg --min-size 2 --max-size 6 --desired-capacity 3` | Update ASG capacity |
| `aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg` | Delete an ASG |
| `aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg --force-delete` | Force delete an ASG (with instances) |
| `aws autoscaling suspend-processes --auto-scaling-group-name my-asg` | Suspend all scaling processes |
| `aws autoscaling resume-processes --auto-scaling-group-name my-asg` | Resume suspended processes |
| `aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg` | View scaling activity history |
| `aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name my-asg --query "AutoScalingGroups[0].Instances"` | List instances in an ASG |

```bash
aws autoscaling describe-auto-scaling-groups
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name my-asg
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-configuration-name my-lc \
  --min-size 1 --max-size 4 --desired-capacity 2 \
  --vpc-zone-identifier "subnet-xxxxxxxx"
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --min-size 2 --max-size 6 --desired-capacity 3
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg --force-delete
aws autoscaling suspend-processes --auto-scaling-group-name my-asg
aws autoscaling resume-processes --auto-scaling-group-name my-asg
aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg
aws autoscaling describe-auto-scaling-groups \
  --auto-scaling-group-name my-asg \
  --query "AutoScalingGroups[0].Instances"
```

---

## Launch Configurations

| Command | Description |
|---|---|
| `aws autoscaling describe-launch-configurations` | List all launch configurations |
| `aws autoscaling describe-launch-configurations --launch-configuration-names my-lc` | Get details for a specific LC |
| `aws autoscaling create-launch-configuration --launch-configuration-name my-lc --image-id ami-xxxxxxxx --instance-type t2.micro --key-name my-key --security-groups sg-xxxxxxxx` | Create a launch configuration |
| `aws autoscaling delete-launch-configuration --launch-configuration-name my-lc` | Delete a launch configuration |

```bash
aws autoscaling describe-launch-configurations
aws autoscaling create-launch-configuration \
  --launch-configuration-name my-lc \
  --image-id ami-xxxxxxxx \
  --instance-type t2.micro \
  --key-name my-key \
  --security-groups sg-xxxxxxxx
aws autoscaling delete-launch-configuration --launch-configuration-name my-lc
```

---

## Scaling Policies

| Command | Description |
|---|---|
| `aws autoscaling put-scaling-policy --auto-scaling-group-name my-asg --policy-name scale-out --policy-type TargetTrackingScaling --target-tracking-configuration "PredefinedMetricSpecification={PredefinedMetricType=ASGAverageCPUUtilization},TargetValue=70.0"` | Create a target tracking policy (CPU) |
| `aws autoscaling put-scaling-policy --auto-scaling-group-name my-asg --policy-name scale-out --policy-type StepScaling --adjustment-type ChangeInCapacity --step-adjustments "Magnitude=1"` | Create a step scaling policy |
| `aws autoscaling put-scaling-policy --auto-scaling-group-name my-asg --policy-name scale-out --policy-type SimpleScaling --adjustment-type ChangeInCapacity --scaling-adjustment 1` | Create a simple scaling policy |
| `aws autoscaling describe-policies --auto-scaling-group-name my-asg` | List scaling policies for an ASG |
| `aws autoscaling delete-policy --auto-scaling-group-name my-asg --policy-name scale-out` | Delete a scaling policy |

```bash
# Target tracking (most common)
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-asg \
  --policy-name scale-out \
  --policy-type TargetTrackingScaling \
  --target-tracking-configuration "PredefinedMetricSpecification={PredefinedMetricType=ASGAverageCPUUtilization},TargetValue=70.0"

# Step scaling
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-asg \
  --policy-name scale-out \
  --policy-type StepScaling \
  --adjustment-type ChangeInCapacity \
  --step-adjustments "Magnitude=1"

# Simple scaling
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-asg \
  --policy-name scale-out \
  --policy-type SimpleScaling \
  --adjustment-type ChangeInCapacity \
  --scaling-adjustment 1

# List & delete
aws autoscaling describe-policies --auto-scaling-group-name my-asg
aws autoscaling delete-policy --auto-scaling-group-name my-asg --policy-name scale-out
```

---

## Scheduled Actions

| Command | Description |
|---|---|
| `aws autoscaling put-scheduled-update-group-action --auto-scaling-group-name my-asg --scheduled-action-name scale-up-morning --recurrence "0 8 * * *" --min-size 4 --max-size 8 --desired-capacity 5` | Schedule a scaling action |
| `aws autoscaling describe-scheduled-actions --auto-scaling-group-name my-asg` | List scheduled actions |
| `aws autoscaling delete-scheduled-action --auto-scaling-group-name my-asg --scheduled-action-name scale-up-morning` | Delete a scheduled action |

```bash
aws autoscaling put-scheduled-update-group-action \
  --auto-scaling-group-name my-asg \
  --scheduled-action-name scale-up-morning \
  --recurrence "0 8 * * *" \
  --min-size 4 --max-size 8 --desired-capacity 5
aws autoscaling describe-scheduled-actions --auto-scaling-group-name my-asg
aws autoscaling delete-scheduled-action \
  --auto-scaling-group-name my-asg \
  --scheduled-action-name scale-up-morning
```

---

## Instance Refresh

| Command | Description |
|---|---|
| `aws autoscaling start-instance-refresh --auto-scaling-group-name my-asg` | Start an instance refresh (rolling update) |
| `aws autoscaling describe-instance-refreshes --auto-scaling-group-name my-asg` | Check instance refresh status |

```bash
aws autoscaling start-instance-refresh --auto-scaling-group-name my-asg
aws autoscaling describe-instance-refreshes --auto-scaling-group-name my-asg
```

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
