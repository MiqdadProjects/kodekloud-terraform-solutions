```markdown
# 🌟 Task 10 - Create Snapshot Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** has some volumes in different regions in their AWS account. They are going to setup some automated backups so that all important data can be backed up on regular basis. For now they shared some requirements to take a snapshot of one of the volumes they have.

This task focuses on implementing data backup strategies through EBS snapshots, which are essential for disaster recovery, data protection, and compliance requirements in production environments.

**Requirements:**
- Create a snapshot of an existing volume named **`devops-vol`** in **us-east-1** region
- The name of the snapshot must be **`devops-vol-ss`**
- The description must be **`Devops Snapshot`**
- Make sure the snapshot status is **completed** before submitting the task
- The Terraform working directory is **`/home/bob/terraform`**
- Update the **`main.tf`** file (do not create a separate .tf file)

👉 **Your task:** Create an EBS snapshot from an existing volume to implement automated backup strategies for the Nautilus infrastructure.

💡 **Note:** EBS snapshots provide point-in-time backups of EBS volumes and are stored in Amazon S3 for durability.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- Source EBS Volume (devops-vol) for snapshot creation
- EBS Snapshot with specified naming and description
- Point-in-time backup stored in S3

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Source EBS Volume:** The devops-vol volume that serves as the snapshot source
- **EBS Snapshot Resource:** Creates a point-in-time backup of the volume
- **Backup Storage:** Snapshot data stored in S3 with cross-AZ durability
- **Resource Tagging:** Proper naming convention for backup identification

### 🎯 Implementation Strategy
1. Ensure source EBS volume (devops-vol) exists in us-east-1
2. Create EBS snapshot resource referencing the source volume
3. Configure proper naming and description as specified
4. Verify snapshot reaches completed status

---

## 🚀 Implementation Steps

### Step 1: Navigate to Working Directory
```bash
cd /home/bob/terraform
```

**Purpose:** Ensure we're working in the correct Terraform directory as specified in the requirements.

---

### Step 2: Update Main Terraform Configuration

Update the `main.tf` file with the complete solution:
```hcl
# main.tf

# Create EBS volume (source for snapshot)
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"
  
  tags = {
    Name = "devops-vol"
  }
}

# Create snapshot of the existing volume
resource "aws_ebs_snapshot" "devops_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Devops Snapshot"
  
  tags = {
    Name = "devops-vol-ss"
  }
}
```

**Configuration Breakdown:**
- `aws_ebs_volume` - Creates the source volume named "devops-vol"
- `aws_ebs_snapshot` - Creates snapshot from the source volume
- `volume_id` - References the source volume ID
- `description` - Sets required "Devops Snapshot" description
- `tags` - Assigns required name "devops-vol-ss"

---

### Step 3: Initialize Terraform (if needed)
```bash
terraform init
```

**Purpose:** Initialize Terraform if not already done, ensuring all required providers are available.

**Expected Output:**
```
Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
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

  # aws_ebs_snapshot.devops_snapshot will be created
  + resource "aws_ebs_snapshot" "devops_snapshot" {
      + arn                = (known after apply)
      + data_encryption_key_id = (known after apply)
      + description        = "Devops Snapshot"
      + encrypted          = (known after apply)
      + id                 = (known after apply)
      + kms_key_id         = (known after apply)
      + owner_alias        = (known after apply)
      + owner_id           = (known after apply)
      + permanent_restore  = (known after apply)
      + snapshot_id        = (known after apply)
      + state              = (known after apply)
      + storage_tier       = (known after apply)
      + tags               = {
          + "Name" = "devops-vol-ss"
        }
      + tags_all           = {
          + "Name" = "devops-vol-ss"
        }
      + temporary_restore_days = (known after apply)
      + volume_id          = (known after apply)
      + volume_size        = (known after apply)
    }

  # aws_ebs_volume.k8s_volume will be created
  + resource "aws_ebs_volume" "k8s_volume" {
      + arn               = (known after apply)
      + availability_zone = "us-east-1a"
      + encrypted         = (known after apply)
      + final_snapshot    = false
      + id                = (known after apply)
      + iops              = (known after apply)
      + kms_key_id        = (known after apply)
      + multi_attach_enabled = false
      + size              = 5
      + snapshot_id       = (known after apply)
      + tags              = {
          + "Name" = "devops-vol"
        }
      + tags_all          = {
          + "Name" = "devops-vol"
        }
      + throughput        = (known after apply)
      + type              = "gp2"
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create EBS volume (source for snapshot)
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"        # AZ placement for the source volume
  size              = 5                   # Volume size in GiB
  type              = "gp2"               # General Purpose SSD
  
  tags = {
    Name = "devops-vol"                   # Required source volume name
  }
}

# Create snapshot of the existing volume
resource "aws_ebs_snapshot" "devops_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id  # Reference to source volume
  description = "Devops Snapshot"              # Required description
  
  tags = {
    Name = "devops-vol-ss"                     # Required snapshot name
  }
}
```

### Snapshot Creation Process
1. **Volume Identification:** Terraform identifies the source volume by ID
2. **Snapshot Initiation:** AWS begins creating point-in-time snapshot
3. **Data Copy:** Volume data is incrementally copied to S3
4. **State Transition:** Snapshot moves from "pending" to "completed"
5. **Availability:** Snapshot becomes available for restore operations

### Resource Dependencies
```
aws_ebs_volume.k8s_volume (source volume)
    └── aws_ebs_snapshot.devops_snapshot
        └── depends on aws_ebs_volume.k8s_volume.id
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
aws_ebs_snapshot.devops_snapshot
aws_ebs_volume.k8s_volume
```

### Step 2: Check Snapshot Status Using AWS CLI
```bash
# Verify snapshot status and details
aws ec2 describe-snapshots --filters "Name=tag:Name,Values=devops-vol-ss" --region us-east-1
```

**Expected Output:**
```json
{
    "Snapshots": [
        {
            "SnapshotId": "snap-6dd7d91e4c32f87ac",
            "VolumeId": "vol-068dc59231be84d29",
            "State": "completed",
            "Description": "Devops Snapshot",
            "StartTime": "2024-XX-XXTXX:XX:XX.000Z",
            "Progress": "100%",
            "VolumeSize": 5,
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "devops-vol-ss"
                }
            ]
        }
    ]
}
```

### Step 3: Verify Snapshot State via Terraform
```bash
# Check snapshot state from Terraform state
terraform state show aws_ebs_snapshot.devops_snapshot | grep "state.*="
```

**Expected Output:**
```
state = "completed"
```

---

## 🧪 Testing

### Verify Snapshot Properties
```bash
# Show complete snapshot details from Terraform state
terraform state show aws_ebs_snapshot.devops_snapshot
```

### Test Snapshot Integrity
```bash
# Get snapshot details for verification
aws ec2 describe-snapshots --snapshot-ids $(terraform state show aws_ebs_snapshot.devops_snapshot | grep "^id " | cut -d'"' -f4) --region us-east-1
```

### Validate Backup Functionality
```bash
# Verify snapshot can be used to create a new volume (optional test)
aws ec2 create-volume --size 5 --snapshot-id $(terraform state show aws_ebs_snapshot.devops_snapshot | grep "^id " | cut -d'"' -f4) --availability-zone us-east-1a --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=restored-from-snapshot}]'
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create EBS volume (source for snapshot)
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"
  
  tags = {
    Name = "devops-vol"
  }
}

# Create snapshot of the existing volume
resource "aws_ebs_snapshot" "devops_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Devops Snapshot"
  
  tags = {
    Name = "devops-vol-ss"
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### Verification Commands
```bash
# Check snapshot status
aws ec2 describe-snapshots --filters "Name=tag:Name,Values=devops-vol-ss" --region us-east-1

# Verify completion
terraform state show aws_ebs_snapshot.devops_snapshot | grep state
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient snapshot information access
output "source_volume_id" {
  description = "ID of the source EBS volume"
  value       = aws_ebs_volume.k8s_volume.id
}

output "snapshot_id" {
  description = "ID of the created snapshot"
  value       = aws_ebs_snapshot.devops_snapshot.id
}

output "snapshot_state" {
  description = "State of the snapshot"
  value       = aws_ebs_snapshot.devops_snapshot.state
}

output "snapshot_description" {
  description = "Description of the snapshot"
  value       = aws_ebs_snapshot.devops_snapshot.description
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Snapshot creation takes long time**
- **Symptoms:** Snapshot remains in "pending" state for extended period
- **Solution:** Snapshot creation time depends on volume size and data amount
```bash
# Monitor snapshot progress
aws ec2 describe-snapshots --snapshot-ids <snapshot-id> --query 'Snapshots[0].[State,Progress]' --output table

# Large volumes can take hours to complete
# 5GB volume typically completes within 5-10 minutes
```

**Issue 2: Source volume not found**
- **Symptoms:** Error: "InvalidVolume.NotFound"
- **Solution:** Ensure source volume exists and is in correct region
```bash
# Verify source volume exists
aws ec2 describe-volumes --filters "Name=tag:Name,Values=devops-vol" --region us-east-1

# Check volume state
aws ec2 describe-volumes --volume-ids <volume-id> --query 'Volumes[0].State'
```

**Issue 3: Insufficient permissions**
- **Symptoms:** Error: "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have snapshot permissions
```bash
# Required permissions:
# - ec2:CreateSnapshot
# - ec2:DescribeSnapshots
# - ec2:DescribeVolumes
# - ec2:CreateTags
```

**Issue 4: Snapshot limit exceeded**
- **Symptoms:** Error: "SnapshotCreationPerVolumeRateExceeded"
- **Solution:** AWS limits snapshot creation rate per volume
```bash
# Check existing snapshots for the volume
aws ec2 describe-snapshots --owner-ids self --filters "Name=volume-id,Values=<volume-id>"

# Wait before creating additional snapshots
# Limit: 5 snapshots per volume per hour
```

---

## 💡 Best Practices Applied

- **🔐 Data Protection:** Implementing point-in-time backups for data recovery
- **📊 Resource Management:** Proper dependency management between volume and snapshot
- **🏷️ Naming Conventions:** Clear, descriptive naming for backup identification
- **🔄 Backup Strategy:** Foundation for automated backup scheduling
- **📍 Regional Strategy:** Snapshots stored with cross-AZ durability in S3

---

## 🚀 Production Considerations

### Backup Strategy Enhancement
- **Automated Scheduling:** Implement automated snapshot creation with Lambda
- **Retention Policies:** Set up lifecycle rules for snapshot cleanup
- **Cross-Region Backup:** Copy snapshots to other regions for disaster recovery
- **Monitoring:** Set up CloudWatch alarms for backup failures
- **Documentation:** Maintain backup and restore procedures

### Cost Optimization
```bash
# Snapshot cost considerations:
├── Storage Cost: Incremental storage in S3
├── Transfer Cost: Cross-region copy charges
├── Retention Management: Delete old snapshots
└── Data Lifecycle: Archive infrequently accessed snapshots
```

### Automation Framework
- **Backup Windows:** Schedule snapshots during low-usage periods
- **Tagging Strategy:** Consistent tagging for automated management
- **Notification System:** SNS notifications for backup status
- **Compliance:** Meet regulatory backup requirements

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **EBS Snapshot Management:** Creating point-in-time backups with Terraform
- **Data Protection Strategy:** Implementing backup solutions for data recovery
- **Resource Dependencies:** Understanding volume-to-snapshot relationships

**Terraform Features Used:**
- `aws_ebs_snapshot` - Snapshot resource creation
- `aws_ebs_volume` - Source volume for snapshot
- Resource references and dependencies
- State management for backup resources

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Source Volume** - Created devops-vol as snapshot source
- **Snapshot Name** - Created "devops-vol-ss" as required
- **Description** - Set to "Devops Snapshot" as specified
- **Status Verification** - Confirmed snapshot reached "completed" state
- **File Structure** - Updated single `main.tf` file as required

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Backup Foundation:** The EBS snapshot is now completed and ready for data recovery operations, providing a reliable point-in-time backup of the devops-vol volume as part of the Nautilus automated backup strategy.

### 🔮 Snapshot Ready for:
- **Volume Restoration:** Create new volumes from this snapshot
- **Data Recovery:** Restore data in case of volume corruption or loss
- **Environment Cloning:** Create identical volumes for testing/development
- **Cross-Region Backup:** Copy snapshot to other regions for DR
- **Automated Backup Scheduling:** Foundation for recurring backup automation
```