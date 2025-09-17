```markdown
# 🌟 Task 11 - Create CloudWatch Alarm Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is setting up monitoring in their AWS account. As part of this, they need to create a CloudWatch alarm to monitor their infrastructure and ensure optimal performance and availability.

CloudWatch alarms are essential for proactive monitoring, allowing teams to detect and respond to performance issues before they impact users or business operations.

**Requirements:**
- Create a CloudWatch alarm named **`datacenter-alarm`**
- The alarm should monitor **CPU utilization** of an EC2 instance
- Trigger the alarm when CPU utilization **exceeds 80%**
- Set the evaluation period to **5 minutes**
- Use a **single evaluation period**
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Implement comprehensive CloudWatch monitoring for EC2 instances to enable proactive infrastructure management.

💡 **Note:** CloudWatch alarms provide automated monitoring and can trigger actions like notifications or auto-scaling when thresholds are breached.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (default region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- EC2 Instance for monitoring target
- CloudWatch Metric Alarm for CPU utilization monitoring
- Automated threshold-based alerting system

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **EC2 Instance:** Target instance for CPU utilization monitoring
- **CloudWatch Metric Alarm:** Monitors CPUUtilization metric from AWS/EC2 namespace
- **Threshold Configuration:** 80% CPU utilization trigger with 5-minute evaluation
- **Monitoring Integration:** Seamless integration with AWS CloudWatch service

### 🎯 Implementation Strategy
1. Create EC2 instance as monitoring target
2. Configure CloudWatch alarm for CPU utilization monitoring
3. Set threshold at 80% with 5-minute evaluation period
4. Use single evaluation period for immediate alerting
5. Configure alarm parameters for production monitoring

---

## 🚀 Implementation Steps

### Step 1: Navigate to Working Directory
```bash
cd /home/bob/terraform
```

**Purpose:** Ensure we're working in the correct Terraform directory as specified in the requirements.

---

### Step 2: Create Main Terraform Configuration

Create the `main.tf` file with the complete solution:
```hcl
# main.tf

# Create EC2 instance for monitoring
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-12345678"   # Placeholder AMI for LocalStack/testing
  instance_type = "t2.micro"
  
  tags = {
    Name = "datacenter-ec2"
  }
}

# Create CloudWatch alarm for CPU monitoring
resource "aws_cloudwatch_metric_alarm" "datacenter_alarm" {
  alarm_name          = "datacenter-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "This alarm triggers when CPU utilization exceeds 80%."
  actions_enabled     = false  # Disable actions since SNS may not be configured
  
  dimensions = {
    InstanceId = aws_instance.datacenter_ec2.id
  }
}
```

**Configuration Breakdown:**
- `aws_instance` - Creates target EC2 instance for monitoring
- `aws_cloudwatch_metric_alarm` - Configures CPU utilization alarm
- `comparison_operator` - "GreaterThanThreshold" for exceeding 80%
- `evaluation_periods = 1` - Single evaluation period as required
- `period = 300` - 5 minutes (300 seconds) evaluation period
- `threshold = 80` - 80% CPU utilization trigger point

---

### Step 3: Initialize Terraform
```bash
terraform init
```

**Purpose:** Initialize the Terraform working directory and download required providers.

**Expected Output:**
```
Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v5.20.0...
- Using previously-installed hashicorp/aws v5.20.0

Terraform has been successfully initialized!
```

---

### Step 4: Format and Validate Configuration
```bash
# Format the configuration
terraform fmt

# Validate the configuration
terraform validate
```

**Purpose:** Ensure code formatting consistency and validate syntax correctness.

**Expected Output:**
```
Success! The configuration is valid.
```

---

### Step 5: Plan and Apply Configuration
```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply -auto-approve
```

**Expected Output:**
```
Terraform will perform the following actions:

  # aws_cloudwatch_metric_alarm.datacenter_alarm will be created
  + resource "aws_cloudwatch_metric_alarm" "datacenter_alarm" {
      + actions_enabled                       = false
      + alarm_actions                         = (known after apply)
      + alarm_description                     = "This alarm triggers when CPU utilization exceeds 80%."
      + alarm_name                            = "datacenter-alarm"
      + arn                                   = (known after apply)
      + comparison_operator                   = "GreaterThanThreshold"
      + dimensions                            = (known after apply)
      + evaluate_low_sample_count_percentiles = (known after apply)
      + evaluation_periods                    = 1
      + id                                    = (known after apply)
      + metric_name                           = "CPUUtilization"
      + namespace                             = "AWS/EC2"
      + ok_actions                            = (known after apply)
      + period                                = 300
      + statistic                             = "Average"
      + tags_all                              = (known after apply)
      + threshold                             = 80
      + treat_missing_data                    = "missing"
    }

  # aws_instance.datacenter_ec2 will be created
  + resource "aws_instance" "datacenter_ec2" {
      + ami                                  = "ami-12345678"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns_name                     = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns_name                      = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "datacenter-ec2"
        }
      + tags_all                             = {
          + "Name" = "datacenter-ec2"
        }
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create EC2 instance for monitoring
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-12345678"           # Placeholder AMI ID
  instance_type = "t2.micro"              # Free tier eligible instance type
  
  tags = {
    Name = "datacenter-ec2"               # Instance identifier
  }
}

# Create CloudWatch alarm for CPU monitoring
resource "aws_cloudwatch_metric_alarm" "datacenter_alarm" {
  alarm_name          = "datacenter-alarm"        # Required alarm name
  comparison_operator = "GreaterThanThreshold"    # Trigger when exceeding threshold
  evaluation_periods  = 1                         # Single evaluation period
  metric_name         = "CPUUtilization"          # AWS EC2 CPU metric
  namespace           = "AWS/EC2"                 # EC2 metrics namespace
  period              = 300                       # 5 minutes (300 seconds)
  statistic           = "Average"                 # Average CPU over period
  threshold           = 80                        # 80% CPU threshold
  alarm_description   = "This alarm triggers when CPU utilization exceeds 80%."
  actions_enabled     = false                     # Disable actions for setup
  
  dimensions = {
    InstanceId = aws_instance.datacenter_ec2.id   # Target specific EC2 instance
  }
}
```

### CloudWatch Alarm Parameters Explained
| Parameter | Value | Description |
|-----------|--------|-------------|
| **alarm_name** | datacenter-alarm | Unique identifier for the alarm |
| **comparison_operator** | GreaterThanThreshold | Triggers when metric exceeds threshold |
| **evaluation_periods** | 1 | Number of periods to evaluate |
| **metric_name** | CPUUtilization | AWS EC2 CPU utilization metric |
| **namespace** | AWS/EC2 | CloudWatch namespace for EC2 metrics |
| **period** | 300 | Evaluation period in seconds (5 minutes) |
| **statistic** | Average | Statistical aggregation method |
| **threshold** | 80 | Threshold value (80%) |

### Monitoring Architecture
```
EC2 Instance (datacenter-ec2)
    ├── CloudWatch Agent (automatic)
    └── CPUUtilization Metric
        └── CloudWatch Alarm (datacenter-alarm)
            ├── Threshold: 80%
            ├── Period: 5 minutes
            └── Evaluation: 1 period
```

---

## ✅ Verification Steps

### Step 1: Verify Resources Created
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_cloudwatch_metric_alarm.datacenter_alarm
aws_instance.datacenter_ec2
```

### Step 2: Check CloudWatch Alarm Status
```bash
# Show alarm details from Terraform state
terraform state show aws_cloudwatch_metric_alarm.datacenter_alarm
```

### Step 3: Verify Alarm in AWS Console (Optional)
```bash
# List CloudWatch alarms using AWS CLI
aws cloudwatch describe-alarms --alarm-names datacenter-alarm
```

**Expected JSON Output:**
```json
{
    "MetricAlarms": [
        {
            "AlarmName": "datacenter-alarm",
            "ComparisonOperator": "GreaterThanThreshold",
            "EvaluationPeriods": 1,
            "MetricName": "CPUUtilization",
            "Namespace": "AWS/EC2",
            "Period": 300,
            "Statistic": "Average",
            "Threshold": 80.0,
            "AlarmDescription": "This alarm triggers when CPU utilization exceeds 80%.",
            "StateValue": "INSUFFICIENT_DATA",
            "Dimensions": [
                {
                    "Name": "InstanceId",
                    "Value": "i-xxxxxxxxxxxxxxxxx"
                }
            ]
        }
    ]
}
```

---

## 🧪 Testing

### Verify Alarm Configuration
```bash
# Show complete alarm details from Terraform state
terraform state show aws_cloudwatch_metric_alarm.datacenter_alarm
```

### Test Alarm Functionality (Optional)
```bash
# Generate CPU load on the instance to test alarm (if accessible)
# ssh to instance and run: stress --cpu 1 --timeout 600s

# Monitor alarm state changes
aws cloudwatch describe-alarms --alarm-names datacenter-alarm --query 'MetricAlarms[0].StateValue'
```

### Validate Monitoring Setup
```bash
# Check if EC2 instance is generating metrics
aws cloudwatch get-metric-statistics \
    --namespace AWS/EC2 \
    --metric-name CPUUtilization \
    --dimensions Name=InstanceId,Value=$(terraform state show aws_instance.datacenter_ec2 | grep "^id " | cut -d'"' -f4) \
    --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) \
    --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
    --period 300 \
    --statistics Average
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create EC2 instance for monitoring
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  
  tags = {
    Name = "datacenter-ec2"
  }
}

# Create CloudWatch alarm for CPU monitoring
resource "aws_cloudwatch_metric_alarm" "datacenter_alarm" {
  alarm_name          = "datacenter-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "This alarm triggers when CPU utilization exceeds 80%."
  actions_enabled     = false
  
  dimensions = {
    InstanceId = aws_instance.datacenter_ec2.id
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### Monitoring Commands
```bash
# Check alarm status
aws cloudwatch describe-alarms --alarm-names datacenter-alarm

# View alarm history
aws cloudwatch describe-alarm-history --alarm-name datacenter-alarm
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient monitoring information
output "instance_id" {
  description = "ID of the monitored EC2 instance"
  value       = aws_instance.datacenter_ec2.id
}

output "alarm_name" {
  description = "Name of the CloudWatch alarm"
  value       = aws_cloudwatch_metric_alarm.datacenter_alarm.alarm_name
}

output "alarm_arn" {
  description = "ARN of the CloudWatch alarm"
  value       = aws_cloudwatch_metric_alarm.datacenter_alarm.arn
}

output "threshold" {
  description = "CPU utilization threshold"
  value       = "${aws_cloudwatch_metric_alarm.datacenter_alarm.threshold}%"
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Alarm remains in INSUFFICIENT_DATA state**
- **Symptoms:** Alarm shows INSUFFICIENT_DATA instead of OK
- **Solution:** Wait for EC2 instance to generate metrics (5-15 minutes)
```bash
# Check if instance is running and generating metrics
aws ec2 describe-instances --instance-ids <instance-id> --query 'Reservations[*].Instances[*].State.Name'

# Verify CloudWatch agent is collecting metrics
aws logs describe-log-groups --log-group-name-prefix /aws/ec2
```

**Issue 2: Invalid AMI ID error**
- **Symptoms:** Error: "InvalidAMIID.NotFound"
- **Solution:** Use valid AMI ID for your region
```bash
# Find valid Amazon Linux AMI in current region
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm-*" --query 'Images[0].ImageId' --output text

# Update main.tf with valid AMI ID
```

**Issue 3: CloudWatch permissions error**
- **Symptoms:** Error: "UnauthorizedOperation" for CloudWatch operations
- **Solution:** Ensure IAM permissions for CloudWatch
```bash
# Required permissions:
# - cloudwatch:PutMetricAlarm
# - cloudwatch:DescribeAlarms
# - cloudwatch:GetMetricStatistics
# - ec2:DescribeInstances
```

**Issue 4: Alarm not triggering**
- **Symptoms:** CPU exceeds 80% but alarm doesn't trigger
- **Solution:** Check alarm configuration and metric availability
```bash
# Verify alarm configuration
aws cloudwatch describe-alarms --alarm-names datacenter-alarm --query 'MetricAlarms[0].[ComparisonOperator,Threshold,EvaluationPeriods]'

# Check recent metric data
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=<instance-id> --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) --end-time $(date -u +%Y-%m-%dT%H:%M:%S) --period 300 --statistics Average
```

---

## 💡 Best Practices Applied

- **🔐 Monitoring Security:** Comprehensive CPU monitoring without exposing sensitive data
- **📊 Resource Management:** Efficient alarm configuration with minimal overhead
- **🏷️ Naming Conventions:** Clear, descriptive naming for monitoring resources
- **🔄 Threshold Management:** Appropriate 80% threshold for production workloads
- **📍 Single Point Monitoring:** Focused monitoring on specific EC2 instance

---

## 🚀 Production Considerations

### Enhanced Monitoring Strategy
- **Multiple Metrics:** Monitor memory, disk, and network alongside CPU
- **SNS Integration:** Configure alarm actions to send notifications
- **Auto Scaling:** Integrate with Auto Scaling Groups for automatic responses
- **Dashboard Creation:** Build CloudWatch dashboards for visualization
- **Log Integration:** Correlate alarms with application logs

### Alarm Actions Configuration
```hcl
# Production alarm with SNS notification
resource "aws_sns_topic" "alerts" {
  name = "infrastructure-alerts"
}

resource "aws_cloudwatch_metric_alarm" "enhanced_alarm" {
  alarm_name          = "datacenter-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2                    # Multiple periods for stability
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "CPU utilization exceeds 80%"
  actions_enabled     = true
  
  alarm_actions = [aws_sns_topic.alerts.arn]  # Send notifications
  ok_actions    = [aws_sns_topic.alerts.arn]  # Send recovery notifications
  
  dimensions = {
    InstanceId = aws_instance.datacenter_ec2.id
  }
}
```

### Monitoring Best Practices
- **Threshold Tuning:** Adjust thresholds based on application behavior
- **Evaluation Periods:** Use multiple periods to reduce false alarms
- **Metric Granularity:** Choose appropriate monitoring intervals
- **Cost Management:** Monitor CloudWatch costs with detailed billing

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **CloudWatch Monitoring:** Setting up automated infrastructure monitoring
- **Metric Alarms:** Configuring threshold-based alerting systems
- **EC2 Monitoring Integration:** Monitoring EC2 instances with CloudWatch

**Terraform Features Used:**
- `aws_cloudwatch_metric_alarm` - CloudWatch alarm creation
- `aws_instance` - EC2 instance for monitoring target
- Resource dependencies and metric integration
- Monitoring configuration management

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Alarm Name** - Created "datacenter-alarm" as required
- **CPU Monitoring** - Configured CPUUtilization metric monitoring
- **Threshold** - Set 80% CPU utilization trigger point
- **Evaluation Period** - Configured 5-minute evaluation period
- **Single Period** - Used 1 evaluation period as specified
- **File Structure** - Created single `main.tf` file as required

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Monitoring Foundation:** The CloudWatch alarm is now active and monitoring CPU utilization of the datacenter-ec2 instance, providing proactive alerting when CPU usage exceeds 80% over a 5-minute period as part of the Nautilus monitoring strategy.

### 🔮 Monitoring Ready for:
- **Proactive Alerting:** Immediate notification when thresholds are breached
- **Performance Optimization:** Identify and resolve performance bottlenecks
- **Capacity Planning:** Historical data analysis for infrastructure scaling
- **Incident Response:** Automated alerts for rapid issue resolution
- **SLA Monitoring:** Track performance against service level agreements
```