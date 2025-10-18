# 🌟 Task 30 - Delete AWS EC2 Instance Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is performing a cleanup process to remove temporary resources from their AWS account as part of a migration effort. An EC2 instance used for testing is no longer needed and must be deleted, while retaining the provisioning code for potential future use.

**Requirements:**
- Delete the EC2 instance named **`nautilus-ec2`** in the `us-east-1` region using Terraform.
- Retain the provisioning code in the `main.tf` file for potential re-provisioning.
- Ensure the instance is in a **terminated** state before task submission.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update or use the existing **`main.tf`** file (do not create a separate `.tf` file).

💡 **Note:** The EC2 instance `nautilus-ec2` must be terminated, but the Terraform configuration should remain intact for future use. Verification of the terminated state is required. The current date and time is October 18, 2025, 05:35 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (EC2, us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- EC2 instance named `nautilus-ec2`
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **EC2 Instance Resource:** Manages the `nautilus-ec2` instance with specified AMI, instance type, and security group.
- **Terraform Configuration:** Retains the `aws_instance` resource definition in `main.tf` for future provisioning.
- **Cleanup Process:** Uses `terraform destroy` to terminate the instance while preserving the configuration.

### 🎯 Implementation Strategy
1. Verify the existence of the `nautilus-ec2` instance in the `us-east-1` region using AWS CLI.
2. Use the provided `main.tf` configuration to manage the instance.
3. Execute `terraform destroy` to terminate the instance.
4. Verify the instance is in a **terminated** state using AWS CLI.
5. Retain the `main.tf` file unchanged for future provisioning.

---

## 🚀 Implementation Steps

### Step 1: Verify Existing EC2 Instance

Confirm the `nautilus-ec2` instance exists in the `us-east-1` region using AWS CLI.

```bash
# Check the instance details
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2" --query "Reservations[].Instances[].{InstanceId:InstanceId,State:State.Name}"
```

**Example Output:**
```json
[
    {
        "InstanceId": "i-0123456789abcdef0",
        "State": "running"
    }
]
```

**Note:** Ensure AWS CLI is configured with credentials that have EC2 permissions.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Verify Main Terraform Configuration

The provided `main.tf` file contains the configuration for the `nautilus-ec2` instance. Ensure it matches the following:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-a7cf362a3038d0c55"
  ]

  tags = {
    Name = "nautilus-ec2"
  }
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider for the `us-east-1` region.
- `aws_instance.ec2`: Defines the `nautilus-ec2` instance with:
  - AMI: `ami-0c101f26f147fa7fd` (a valid AMI for `us-east-1`).
  - Instance type: `t2.micro`.
  - Security group: `sg-a7cf362a3038d0c55`.
  - Tag: `Name = nautilus-ec2`.

**Note:** The configuration is retained as-is to allow re-provisioning later. No changes are needed to `main.tf`.

---

### Step 4: Initialize Terraform (if not already initialized)

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

### Step 5: Format and Validate Configuration

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

### Step 6: Destroy the EC2 Instance

Execute the `terraform destroy` command to terminate the `nautilus-ec2` instance.

```bash
terraform destroy -auto-approve
```

**Expected Output:**
```
aws_instance.ec2: Destroying... [id=i-0123456789abcdef0]
aws_instance.ec2: Still destroying... [id=i-0123456789abcdef0, 10s elapsed]
aws_instance.ec2: Destruction complete after 20s

Destroy complete! Resources: 1 destroyed.
```

**Note:** The `main.tf` file remains unchanged, preserving the configuration for future use.

---

### Step 7: Verify Instance Termination

Confirm the instance is in a **terminated** state using AWS CLI.

```bash
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2" --query "Reservations[].Instances[].{InstanceId:InstanceId,State:State.Name}"
```

**Expected Output:**
```json
[
    {
        "InstanceId": "i-0123456789abcdef0",
        "State": "terminated"
    }
]
```

**Alternative (if no instances are found):**
```json
[]
```

**Note:** If the instance is not listed or is in the `terminated` state, the cleanup is successful.

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that no resources exist, as the instance has been destroyed.

**Expected Output:**
```
No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-a7cf362a3038d0c55"
  ]

  tags = {
    Name = "nautilus-ec2"
  }
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_instance.ec2.ami** | ami-0c101f26f147fa7fd | AMI ID for the EC2 instance |
| **aws_instance.ec2.instance_type** | t2.micro | Instance type |
| **aws_instance.ec2.vpc_security_group_ids** | sg-a7cf362a3038d0c55 | Security group ID |
| **aws_instance.ec2.tags.Name** | nautilus-ec2 | Tag for instance identification |

### Resource Properties
- **Region:** `us-east-1` as specified.
- **Instance State:** Terminated after `terraform destroy`.
- **Configuration Retention:** The `main.tf` file remains unchanged for future provisioning.

---

## ✅ Verification Steps

### Step 1: Verify Resource Destruction

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_instance.ec2
```

**Note:** The resource remains in the Terraform configuration but not in the state (destroyed).

### Step 2: Check Resource Details in Terraform State

```bash
# Show details of the instance resource
terraform state show aws_instance.ec2
```

**Expected Output:**
```
No resource found
```

**Note:** Since the instance was destroyed, it no longer exists in the Terraform state.

### Step 3: Verify in AWS Console (Optional)

```bash
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2" --query "Reservations[].Instances[].{InstanceId:InstanceId,State:State.Name}"
```

**Expected Output:**
```json
[
    {
        "InstanceId": "i-0123456789abcdef0",
        "State": "terminated"
    }
]
```

**Or (if fully removed):**
```json
[]
```

---

## 🧪 Testing

### Verify Instance Termination

1. Check the instance state:
```bash
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2"
```

**Expected Output:** Either shows the instance in `terminated` state or no instances found.

2. Attempt to re-provision (optional, for validation):
```bash
terraform apply -auto-approve
```

**Expected Output:** Re-creates the `nautilus-ec2` instance, confirming the configuration is intact.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-a7cf362a3038d0c55"
  ]

  tags = {
    Name = "nautilus-ec2"
  }
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
terraform init
terraform destroy -auto-approve
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_instance.ec2

# Verify via AWS CLI
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2" --query "Reservations[].Instances[].{InstanceId:InstanceId,State:State.Name}"
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Instance Not Found**

- **Symptoms:** Error: "No instance found with tag Name=nautilus-ec2"
- **Solution:** Verify the instance exists or was already terminated
```bash
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=nautilus-ec2"
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: ec2:TerminateInstances"
- **Solution:** Ensure AWS credentials have EC2 permissions
```bash
# Required permissions:
# - ec2:DescribeInstances
# - ec2:TerminateInstances
```

**Issue 3: Terraform Destroy Fails**

- **Symptoms:** Error: "Resource still in use or dependent"
- **Solution:** Check for dependencies (e.g., EBS volumes, ENIs) and retry
```bash
aws ec2 describe-instances --region us-east-1 --instance-ids i-0123456789abcdef0
terraform destroy -auto-approve
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Ensured credentials have least-privilege access for EC2 operations.
- **📊 Resource Naming:** Used clear names (`nautilus-ec2`) via tags.
- **🏷️ Minimal Configuration:** Retained the `main.tf` file unchanged for future use.
- **📍 Verifiability:** Ensured termination is verifiable via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### EC2 Cleanup Framework
- **State Management:** Ensure instances are terminated but configurations are preserved.
- **Monitoring:** Enable AWS CloudTrail to log EC2 actions for auditing.
- **Resource Tagging:** Use consistent tagging for cost allocation and tracking.
- **Re-provisioning:** The retained `main.tf` allows quick re-provisioning with `terraform apply`.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-a7cf362a3038d0c55"
  ]

  tags = {
    Name        = "nautilus-ec2"
    Team        = "nautilus-devops"
    Environment = "test"
    Purpose     = "migration-testing"
  }
}

output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.ec2.id
}
```

### Security Strategy
- **Access Control:** Use IAM roles with specific EC2 permissions.
- **Monitoring:** Log EC2 actions with CloudTrail for audit trails.
- **Cost Optimization:** Terminate unused instances to reduce costs.
- **Validation:** Verify AMI and security group IDs before re-provisioning.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **EC2 Management:** Terminating EC2 instances with Terraform.
- **Configuration Retention:** Preserving Terraform code for future provisioning.
- **Resource Cleanup:** Safely removing cloud resources while maintaining configurations.
- **State Verification:** Confirming resource termination using AWS CLI.

**Terraform Features Used:**
- `aws_instance`: Managing the EC2 instance.
- `terraform destroy`: Removing resources while retaining configuration.
- AWS provider integration for EC2 services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Instance Terminated**: Deleted the `nautilus-ec2` instance in `us-east-1`.
- **Configuration Retained**: Kept the `main.tf` file unchanged for future provisioning.
- **Verification**: Confirmed the instance is in a **terminated** state via AWS CLI.
- **File Structure**: Used single `main.tf` file as specified.
- **Deployment**: Executed `terraform destroy` with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Cleanup Foundation:** The `nautilus-ec2` instance has been terminated, optimizing the Nautilus DevOps team’s AWS environment, with the configuration preserved for future use.

### 🔮 Resources Ready for:
- **Re-provisioning**: Use `terraform apply` to recreate the instance.
- **Monitoring**: Audit EC2 actions with CloudTrail.
- **Cost Management**: Removed unused instance to reduce costs.