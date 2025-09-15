```markdown
# 🌟 Task 8 - Create AMI Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- Create an AMI from an existing EC2 instance named **`datacenter-ec2`**
- Name of the AMI should be **`datacenter-ec2-ami`**
- Make sure AMI is in **available** state
- The Terraform working directory is **`/home/bob/terraform`**
- Update the **`main.tf`** file (do not create a separate .tf file)

👉 **Your task:** Create a custom Amazon Machine Image (AMI) from an existing EC2 instance to enable consistent instance deployments with pre-configured settings.

💡 **Note:** AMIs allow you to create reusable snapshots of configured instances for rapid deployment and scaling.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (default region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- EC2 Instance (datacenter-ec2) as the source for AMI creation
- AMI created from running instance with custom configurations
- Available state AMI ready for launching new instances

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Source EC2 Instance:** The datacenter-ec2 instance that serves as the template
- **AMI from Instance Resource:** Creates an AMI snapshot from the running instance
- **Image State Management:** Ensures AMI reaches available state for deployment
- **Instance Template:** Reusable image for consistent deployments

### 🎯 Implementation Strategy
1. Ensure source EC2 instance (datacenter-ec2) exists and is running
2. Create AMI from the existing instance using aws_ami_from_instance resource
3. Configure proper naming for the AMI (datacenter-ec2-ami)
4. Wait for AMI to reach available state for usage

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

# Provision EC2 instance (source for AMI creation)
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-89060491a8db79a22"
  ]
  
  tags = {
    Name = "datacenter-ec2"
  }
}

# Create AMI from the EC2 instance
resource "aws_ami_from_instance" "datacenter_ec2_ami" {
  name               = "datacenter-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```

**Configuration Breakdown:**
- `aws_instance` - Source EC2 instance that will be used to create the AMI
- `aws_ami_from_instance` - Creates AMI from specified source instance
- `source_instance_id` - References the EC2 instance ID for AMI creation
- `name` - Required AMI name as specified in requirements

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

  # aws_ami_from_instance.datacenter_ec2_ami will be created
  + resource "aws_ami_from_instance" "datacenter_ec2_ami" {
      + architecture         = (known after apply)
      + arn                  = (known after apply)
      + boot_mode           = (known after apply)
      + deprecation_time    = (known after apply)
      + description         = (known after apply)
      + ena_support         = (known after apply)
      + hypervisor          = (known after apply)
      + id                  = (known after apply)
      + image_location      = (known after apply)
      + image_owner_alias   = (known after apply)
      + image_type          = (known after apply)
      + imds_support        = (known after apply)
      + kernel_id           = (known after apply)
      + manage_ebs_snapshots = false
      + name                = "datacenter-ec2-ami"
      + owner_id            = (known after apply)
      + platform            = (known after apply)
      + platform_details    = (known after apply)
      + public              = false
      + ramdisk_id          = (known after apply)
      + root_device_name    = (known after apply)
      + root_device_type    = (known after apply)
      + snapshot_without_reboot = false
      + source_instance_id  = (known after apply)
      + sriov_net_support   = (known after apply)
      + state               = (known after apply)
      + state_reason        = (known after apply)
      + tags_all            = (known after apply)
      + tpm_support         = (known after apply)
      + usage_operation     = (known after apply)
      + virtualization_type = (known after apply)
    }

  # aws_instance.ec2 will be created
  + resource "aws_instance" "ec2" {
      + ami                                  = "ami-0c101f26f147fa7fd"
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
      + vpc_security_group_ids               = [
          + "sg-89060491a8db79a22",
        ]
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Provision EC2 instance (source for AMI creation)
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"        # Base Amazon Linux AMI
  instance_type = "t2.micro"                     # Instance size
  vpc_security_group_ids = [
    "sg-89060491a8db79a22"                      # Specific security group ID
  ]
  
  tags = {
    Name = "datacenter-ec2"                      # Required instance name
  }
}

# Create AMI from the EC2 instance
resource "aws_ami_from_instance" "datacenter_ec2_ami" {
  name               = "datacenter-ec2-ami"      # Required AMI name
  source_instance_id = aws_instance.ec2.id      # Reference to source instance
}
```

### AMI Creation Process
1. **Instance Preparation:** Source instance must be in "running" or "stopped" state
2. **Snapshot Creation:** AWS creates EBS snapshots of all attached volumes
3. **AMI Registration:** AWS registers the AMI with the snapshots
4. **State Transition:** AMI moves from "pending" to "available" state
5. **Ready for Use:** AMI can be used to launch new instances

### Resource Dependencies
```
aws_instance.ec2 (source instance)
    └── aws_ami_from_instance.datacenter_ec2_ami
        ├── depends on aws_instance.ec2.id
        └── waits for instance to be ready
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
aws_ami_from_instance.datacenter_ec2_ami
aws_instance.ec2
```

### Step 2: Check AMI State
```bash
# Show AMI details including state
terraform state show aws_ami_from_instance.datacenter_ec2_ami
```

### Step 3: Verify AMI is Available
```bash
# Check AMI state specifically
terraform state show aws_ami_from_instance.datacenter_ec2_ami | grep "state.*="
```

**Expected Output:**
```
state = "available"
```

### Step 4: Get AMI ID
```bash
# Extract AMI ID for reference
terraform state show aws_ami_from_instance.datacenter_ec2_ami | grep "^id.*="
```

---

## 🧪 Testing

### Verify AMI Creation
```bash
# Show complete AMI details from Terraform state
terraform state show aws_ami_from_instance.datacenter_ec2_ami
```

### AWS CLI Verification
```bash
# Describe the created AMI using AWS CLI
aws ec2 describe-images --owners self --filters "Name=name,Values=datacenter-ec2-ami"
```

**Expected JSON Output:**
```json
{
    "Images": [
        {
            "ImageId": "ami-xxxxxxxxxxxxxxxxx",
            "Name": "datacenter-ec2-ami",
            "State": "available",
            "ImageLocation": "xxxxxxxxxxxx/datacenter-ec2-ami",
            "Public": false,
            "Architecture": "x86_64",
            "ImageType": "machine",
            "RootDeviceType": "ebs",
            "VirtualizationType": "hvm",
            "CreationDate": "2024-XX-XXTXX:XX:XX.000Z"
        }
    ]
}
```

### Test AMI by Launching New Instance
```bash
# Get AMI ID for testing
AMI_ID=$(terraform state show aws_ami_from_instance.datacenter_ec2_ami | grep "^id " | cut -d'"' -f4)

# Launch a test instance using the created AMI (optional)
aws ec2 run-instances --image-id $AMI_ID --instance-type t2.micro --count 1 --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=test-from-ami}]'
```

### Verify AMI Snapshots
```bash
# List EBS snapshots associated with the AMI
aws ec2 describe-images --image-ids $(terraform state show aws_ami_from_instance.datacenter_ec2_ami | grep "^id " | cut -d'"' -f4) --query 'Images[0].BlockDeviceMappings[*].Ebs.SnapshotId' --output table
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Provision EC2 instance (source for AMI creation)
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-89060491a8db79a22"
  ]
  
  tags = {
    Name = "datacenter-ec2"
  }
}

# Create AMI from the EC2 instance
resource "aws_ami_from_instance" "datacenter_ec2_ami" {
  name               = "datacenter-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### AMI Status Check
```bash
# Check if AMI is available
terraform state show aws_ami_from_instance.datacenter_ec2_ami | grep state
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient AMI information access
output "source_instance_id" {
  description = "ID of the source EC2 instance"
  value       = aws_instance.ec2.id
}

output "ami_id" {
  description = "ID of the created AMI"
  value       = aws_ami_from_instance.datacenter_ec2_ami.id
}

output "ami_name" {
  description = "Name of the created AMI"
  value       = aws_ami_from_instance.datacenter_ec2_ami.name
}

output "ami_state" {
  description = "State of the created AMI"
  value       = aws_ami_from_instance.datacenter_ec2_ami.state
}

output "ami_architecture" {
  description = "Architecture of the created AMI"
  value       = aws_ami_from_instance.datacenter_ec2_ami.architecture
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Source instance not ready**
- **Symptoms:** Error: "InvalidInstanceID.NotFound" or instance not ready
- **Solution:** Ensure source instance exists and is in running state
```bash
# Check instance state
aws ec2 describe-instances --instance-ids <instance-id> --query 'Reservations[*].Instances[*].State.Name'

# Wait for instance to be running before creating AMI
```

**Issue 2: AMI creation takes long time**
- **Symptoms:** AMI remains in "pending" state for extended period
- **Solution:** AMI creation can take 10-20 minutes, especially for larger instances
```bash
# Monitor AMI creation progress
aws ec2 describe-images --image-ids <ami-id> --query 'Images[0].State'

# Check for any errors in AMI creation
aws ec2 describe-images --image-ids <ami-id> --query 'Images[0].StateReason'
```

**Issue 3: AMI name already exists**
- **Symptoms:** Error: "InvalidAMIName.Duplicate"
- **Solution:** AMI names must be unique within your account
```bash
# Check existing AMIs with similar names
aws ec2 describe-images --owners self --filters "Name=name,Values=datacenter-ec2-ami"

# Use unique naming or import existing AMI
```

**Issue 4: Insufficient permissions**
- **Symptoms:** Error: "UnauthorizedOperation"
- **Solution:** Ensure IAM permissions for AMI operations
```bash
# Required permissions:
# - ec2:CreateImage
# - ec2:DescribeImages
# - ec2:DescribeInstances
# - ec2:DescribeSnapshots
```

**Issue 5: Security group not found**
- **Symptoms:** Error: "InvalidGroupId.NotFound"
- **Solution:** Verify security group ID exists in the VPC
```bash
# Check if security group exists
aws ec2 describe-security-groups --group-ids sg-89060491a8db79a22
```

---

## 💡 Best Practices Applied

- **🔐 AMI Security:** Creating AMIs from known, configured instances
- **📊 Resource Management:** Proper dependency management between instance and AMI
- **🏷️ Naming Conventions:** Clear, descriptive AMI naming for identification
- **🔄 State Management:** Ensuring AMI reaches available state before use
- **💰 Cost Awareness:** Understanding AMI storage costs (EBS snapshots)

---

## 🚀 Production Considerations

### AMI Management Strategy
- **Version Control:** Implement AMI versioning with date/build stamps
- **Automated Building:** Use CI/CD pipelines to create AMIs automatically
- **Testing:** Validate AMIs before production deployment
- **Cleanup:** Regular cleanup of old AMIs to manage costs
- **Cross-Region:** Copy AMIs to multiple regions for disaster recovery

### AMI Best Practices
```bash
# Production AMI considerations:
├── Security Updates: Keep base images updated
├── Application Pre-installation: Bake in common software
├── Configuration Management: Use tools like Ansible/Chef
├── Image Testing: Automated validation of AMI functionality
└── Documentation: Maintain AMI change logs and documentation
```

### Cost Management
- **Snapshot Costs:** AMIs incur costs for underlying EBS snapshots
- **Cleanup Policies:** Implement lifecycle policies for old AMIs
- **Sharing Strategy:** Share AMIs across accounts instead of duplicating
- **Monitoring:** Track AMI costs with detailed billing reports

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **AMI Creation:** Creating custom machine images from EC2 instances
- **Image Management:** Understanding AMI states and lifecycle
- **Infrastructure Templates:** Using AMIs as deployment templates

**Terraform Features Used:**
- `aws_ami_from_instance` - AMI creation from existing instances
- `aws_instance` - EC2 instance as AMI source
- Resource dependencies and state management
- AMI state monitoring and validation

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Source Instance** - Created datacenter-ec2 instance as AMI source
- **AMI Name** - Created "datacenter-ec2-ami" as required
- **AMI State** - AMI reached "available" state for deployment
- **Resource Dependencies** - Proper dependency chain between instance and AMI
- **File Structure** - Updated single `main.tf` file as specified

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Custom Image Ready:** The AMI is now available for launching new EC2 instances with the pre-configured settings from the datacenter-ec2 source instance, enabling consistent and rapid deployment of standardized server configurations.

### 🔮 AMI Ready for:
- **Instance Scaling:** Launch multiple identical instances quickly
- **Auto Scaling Groups:** Use as launch template base
- **Disaster Recovery:** Rapid instance replacement with known configuration
- **Development Environments:** Consistent development server deployment
- **Cross-Region Deployment:** Copy AMI to other regions for distribution
```