```markdown
# 🌟 Task 9 - Create EBS Volume Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- Name of the volume should be **`devops-volume`**
- Volume type must be **`gp3`**
- Volume size must be **`2 GiB`**
- Ensure the volume is created in **`us-east-1`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create an EBS volume using Terraform to provide persistent block storage for EC2 instances during the infrastructure migration.

💡 **Note:** EBS volumes provide durable, high-performance block storage that can be attached to EC2 instances.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- EBS Volume (gp3 type) with 2 GiB capacity
- Persistent block storage for data retention
- High-performance storage for application workloads

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **EBS Volume Resource:** Creates a General Purpose SSD (gp3) volume
- **Storage Capacity:** 2 GiB of persistent block storage
- **Availability Zone Placement:** Deployed in us-east-1a for regional compliance
- **Resource Tagging:** Proper naming convention for volume identification

### 🎯 Implementation Strategy
1. Define EBS volume resource with gp3 type for optimal performance
2. Configure 2 GiB size as specified in requirements
3. Place volume in us-east-1a availability zone
4. Apply proper tagging for resource management

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

# Create an EBS volume for DevOps workloads
resource "aws_ebs_volume" "devops_volume" {
  availability_zone = "us-east-1a"
  size              = 2
  type              = "gp3"
  
  tags = {
    Name = "devops-volume"
  }
}
```

**Configuration Breakdown:**
- `aws_ebs_volume` - Creates an Elastic Block Store volume
- `availability_zone = "us-east-1a"` - Places volume in us-east-1 region
- `size = 2` - Sets volume capacity to 2 GiB
- `type = "gp3"` - Uses General Purpose SSD (gp3) for optimal performance
- `tags` - Assigns required name "devops-volume" for identification

---

### Step 3: Initialize Terraform
```bash
terraform init
```

**Purpose:** Initialize the Terraform working directory and download the AWS provider.

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

  # aws_ebs_volume.devops_volume will be created
  + resource "aws_ebs_volume" "devops_volume" {
      + arn               = (known after apply)
      + availability_zone = "us-east-1a"
      + encrypted         = (known after apply)
      + final_snapshot    = false
      + id                = (known after apply)
      + iops              = (known after apply)
      + kms_key_id        = (known after apply)
      + multi_attach_enabled = false
      + size              = 2
      + snapshot_id       = (known after apply)
      + tags              = {
          + "Name" = "devops-volume"
        }
      + tags_all          = {
          + "Name" = "devops-volume"
        }
      + throughput        = (known after apply)
      + type              = "gp3"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create an EBS volume for DevOps workloads
resource "aws_ebs_volume" "devops_volume" {
  availability_zone = "us-east-1a"        # AZ within us-east-1 region
  size              = 2                   # Volume size in GiB
  type              = "gp3"               # General Purpose SSD v3
  
  tags = {
    Name = "devops-volume"                # Required volume name
  }
}
```

### EBS Volume Specifications
| Attribute | Value | Description |
|-----------|--------|-------------|
| **Type** | gp3 | Latest generation General Purpose SSD |
| **Size** | 2 GiB | Storage capacity |
| **IOPS** | 3000 (default) | Input/Output operations per second |
| **Throughput** | 125 MiB/s (default) | Data transfer rate |
| **Encryption** | Default (account setting) | Data encryption at rest |

### gp3 Volume Advantages
- **Better Performance:** Higher baseline IOPS (3000) compared to gp2 (100-3000)
- **Cost Efficient:** Lower cost per GiB than gp2
- **Scalable:** IOPS and throughput can be provisioned independently
- **Consistent Performance:** Predictable performance regardless of volume size

---

## ✅ Verification Steps

### Step 1: Verify EBS Volume Creation
```bash
# List Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_ebs_volume.devops_volume
```

### Step 2: Check Volume Details
```bash
# Show detailed volume information
terraform state show aws_ebs_volume.devops_volume
```

### Step 3: Verify Volume State
```bash
# Check volume state specifically
terraform state show aws_ebs_volume.devops_volume | grep "state.*="
```

**Expected Output:**
```
state = "available"
```

### Step 4: Get Volume ID
```bash
# Extract volume ID for reference
terraform state show aws_ebs_volume.devops_volume | grep "^id.*="
```

---

## 🧪 Testing

### Verify Volume Properties
```bash
# Show complete volume details from Terraform state
terraform state show aws_ebs_volume.devops_volume
```

### AWS CLI Verification
```bash
# Describe the created EBS volume
aws ec2 describe-volumes --filters "Name=tag:Name,Values=devops-volume"
```

**Expected JSON Output:**
```json
{
    "Volumes": [
        {
            "VolumeId": "vol-xxxxxxxxxxxxxxxxx",
            "Size": 2,
            "VolumeType": "gp3",
            "State": "available",
            "AvailabilityZone": "us-east-1a",
            "Encrypted": false,
            "Iops": 3000,
            "Throughput": 125,
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "devops-volume"
                }
            ]
        }
    ]
}
```

### Volume Performance Verification
```bash
# Check volume performance characteristics
aws ec2 describe-volumes --volume-ids $(terraform state show aws_ebs_volume.devops_volume | grep "^id " | cut -d'"' -f4) --query 'Volumes[0].[VolumeType,Iops,Throughput,Size]' --output table
```

### Test Volume Availability
```bash
# Verify volume is in available state and ready for attachment
aws ec2 describe-volumes --volume-ids $(terraform state show aws_ebs_volume.devops_volume | grep "^id " | cut -d'"' -f4) --query 'Volumes[0].State' --output text
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create an EBS volume for DevOps workloads
resource "aws_ebs_volume" "devops_volume" {
  availability_zone = "us-east-1a"
  size              = 2
  type              = "gp3"
  
  tags = {
    Name = "devops-volume"
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### Volume Status Check
```bash
# Verify volume is available
terraform state show aws_ebs_volume.devops_volume | grep state
```

### Optional: Enhanced Configuration
```hcl
# Enhanced configuration with additional gp3 options
resource "aws_ebs_volume" "devops_volume" {
  availability_zone = "us-east-1a"
  size              = 2
  type              = "gp3"
  iops              = 3000          # Baseline IOPS for gp3
  throughput        = 125           # Baseline throughput (MiB/s)
  encrypted         = true          # Enable encryption
  
  tags = {
    Name        = "devops-volume"
    Environment = "production"
    Purpose     = "devops-workloads"
    Project     = "nautilus-migration"
  }
}
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient volume information access
output "volume_id" {
  description = "ID of the EBS volume"
  value       = aws_ebs_volume.devops_volume.id
}

output "volume_arn" {
  description = "ARN of the EBS volume"
  value       = aws_ebs_volume.devops_volume.arn
}

output "volume_size" {
  description = "Size of the EBS volume in GiB"
  value       = aws_ebs_volume.devops_volume.size
}

output "volume_type" {
  description = "Type of the EBS volume"
  value       = aws_ebs_volume.devops_volume.type
}

output "volume_availability_zone" {
  description = "Availability zone of the EBS volume"
  value       = aws_ebs_volume.devops_volume.availability_zone
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Availability zone not found**
- **Symptoms:** Error: "InvalidParameterValue: Invalid availability zone"
- **Solution:** Verify availability zone exists in us-east-1 region
```bash
# List available zones in us-east-1
aws ec2 describe-availability-zones --region us-east-1 --query 'AvailabilityZones[*].ZoneName' --output table
```

**Issue 2: Volume size constraints**
- **Symptoms:** Error: "InvalidParameterValue: Invalid volume size"
- **Solution:** Ensure size meets gp3 requirements (1 GiB - 16,384 GiB)
```bash
# gp3 volume size constraints:
# Minimum: 1 GiB
# Maximum: 16,384 GiB (16 TiB)
# Current: 2 GiB (within limits)
```

**Issue 3: Insufficient permissions**
- **Symptoms:** Error: "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have EBS permissions
```bash
# Required permissions:
# - ec2:CreateVolume
# - ec2:DescribeVolumes
# - ec2:CreateTags
```

**Issue 4: Volume limit exceeded**
- **Symptoms:** Error: "VolumeLimitExceeded"
- **Solution:** Check account EBS volume limits
```bash
# Check current volume usage
aws ec2 describe-volumes --query 'length(Volumes)'

# Default limit varies by account age and usage
# Request limit increase through AWS Support if needed
```

**Issue 5: IOPS/Throughput configuration**
- **Symptoms:** Unexpected IOPS or throughput values
- **Solution:** Understand gp3 defaults and limits
```bash
# gp3 Performance specifications:
# Baseline IOPS: 3,000 (can provision up to 16,000)
# Baseline Throughput: 125 MiB/s (can provision up to 1,000 MiB/s)
# IOPS:throughput ratio: 4:1 (minimum)
```

---

## 💡 Best Practices Applied

- **🔐 Volume Security:** Using modern gp3 volume type with latest security features
- **📊 Resource Management:** Proper tagging for identification and cost tracking
- **🏷️ Naming Conventions:** Clear, descriptive naming for resource identification
- **🔄 Performance Optimization:** gp3 provides better performance-to-cost ratio
- **📍 Regional Strategy:** Explicit availability zone placement in us-east-1

---

## 🚀 Production Considerations

### EBS Volume Management
- **Encryption:** Enable encryption for sensitive data workloads
- **Backup Strategy:** Implement automated snapshot schedules
- **Performance Monitoring:** Monitor IOPS and throughput utilization
- **Cost Optimization:** Regular review of volume usage and rightsizing
- **Multi-AZ Strategy:** Consider volume placement for high availability

### Volume Attachment Planning
```bash
# Common attachment scenarios:
├── EC2 Instance Storage: Additional storage for applications
├── Database Storage: Persistent data storage for databases  
├── Log Storage: Centralized logging and audit trails
├── Backup Storage: Temporary storage for backup operations
└── Development Environments: Persistent development workspaces
```

### Performance Tuning
- **IOPS Scaling:** Monitor and adjust IOPS based on workload requirements
- **Throughput Optimization:** Tune throughput for sequential workloads
- **Instance Optimization:** Ensure EC2 instance supports EBS optimization
- **File System:** Choose appropriate file system (ext4, xfs) for workload

### Cost Management
- **Volume Sizing:** Right-size volumes to avoid over-provisioning
- **Snapshot Lifecycle:** Implement automated snapshot cleanup policies
- **Performance Monitoring:** Avoid over-provisioning IOPS and throughput
- **Reserved Capacity:** Consider EBS Reserved Instances for predictable workloads

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **EBS Volume Management:** Creating persistent block storage with Terraform
- **gp3 Volume Type:** Understanding modern EBS volume performance characteristics
- **Availability Zone Placement:** Strategic volume placement for regional compliance

**Terraform Features Used:**
- `aws_ebs_volume` - EBS volume resource creation
- Volume configuration attributes (size, type, AZ)
- Resource tagging and identification
- AWS provider resource management

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Volume Name** - Created "devops-volume" as required
- **Volume Type** - Configured gp3 type for optimal performance
- **Volume Size** - Set to 2 GiB as specified
- **Region Placement** - Deployed in us-east-1a within us-east-1 region
- **File Structure** - Used single `main.tf` file as required

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Storage Foundation:** The EBS volume is now available for attachment to EC2 instances, providing persistent, high-performance block storage for DevOps workloads during the Nautilus infrastructure migration.

### 🔮 Volume Ready for:
- **EC2 Attachment:** Mount as additional storage on EC2 instances
- **Database Storage:** Persistent storage for database applications
- **Application Data:** Storage for application files and configurations
- **Backup Operations:** Temporary storage for backup and recovery processes
- **Development Workloads:** Persistent development environment storage
```