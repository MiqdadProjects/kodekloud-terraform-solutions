# 🌟 Task 33 - Delete AWS VPC Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is undertaking a phased migration of their infrastructure to the AWS cloud. As part of this process, some resources created in different regions are no longer needed and can be deleted to optimize the environment. A Virtual Private Cloud (VPC) in the `us-east-1` region is targeted for removal, while its provisioning code must be retained for potential future use.

**Requirements:**
- Delete the VPC named **`datacenter-vpc`** in the `us-east-1` region using Terraform.
- Retain the provisioning code in the `main.tf` file for potential re-provisioning.
- Ensure the VPC is deleted before task submission.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update or use the existing **`main.tf`** file (do not create a separate `.tf` file).

💡 **Note:** The VPC `datacenter-vpc` must be deleted, but the Terraform configuration should remain intact for future use. Verification of the deletion is required. The current date and time is October 18, 2025, 05:52 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (VPC, us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- VPC named `datacenter-vpc`
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **VPC Resource:** Manages the `datacenter-vpc` with a specified CIDR block.
- **Terraform Configuration:** Retains the `aws_vpc` resource definition in `main.tf` for future provisioning.
- **Cleanup Process:** Uses `terraform destroy` with a targeted resource to delete the VPC while preserving the configuration.

### 🎯 Implementation Strategy
1. Verify the existence of the `datacenter-vpc` VPC in the `us-east-1` region using AWS CLI.
2. Use the provided `main.tf` configuration to manage the VPC.
3. Execute `terraform destroy` with a target to delete the VPC.
4. Verify the VPC is deleted using AWS CLI.
5. Retain the `main.tf` file unchanged for future provisioning.

---

## 🚀 Implementation Steps

### Step 1: Verify Existing VPC

Confirm the `datacenter-vpc` exists in the `us-east-1` region using AWS CLI.

```bash
# Check the VPC details
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc" --query "Vpcs[].{VpcId:VpcId,CidrBlock:CidrBlock}"
```

**Example Output:**
```json
[
    {
        "VpcId": "vpc-0123456789abcdef0",
        "CidrBlock": "10.0.0.0/16"
    }
]
```

**Note:** Ensure AWS CLI is configured with credentials that have VPC permissions. If the VPC has dependencies (e.g., subnets, route tables, internet gateways), they must be removed before deletion.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Verify Main Terraform Configuration

The provided `main.tf` file contains the configuration for the `datacenter-vpc`. Ensure it matches the following:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision VPC
resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider for the `us-east-1` region.
- `aws_vpc.this`: Defines the `datacenter-vpc` with a CIDR block of `10.0.0.0/16`.
- `tags`: Applies the name `datacenter-vpc` for identification.
- The configuration is retained as-is to allow re-provisioning later. No changes are needed to `main.tf`.

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

### Step 6: Destroy the VPC

Execute the `terraform destroy` command with a target to delete the `datacenter-vpc`.

```bash
terraform destroy -target=aws_vpc.this -auto-approve
```

**Expected Output:**
```
aws_vpc.this: Destroying... [id=vpc-0123456789abcdef0]
aws_vpc.this: Destruction complete after 1s

Destroy complete! Resources: 1 destroyed.
```

**Note:** The `main.tf` file remains unchanged, preserving the configuration for future use. The `-target` flag ensures only the specified resource is deleted.

---

### Step 7: Verify VPC Deletion

Confirm the VPC is deleted using AWS CLI.

```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc" --query "Vpcs[].{VpcId:VpcId,CidrBlock:CidrBlock}"
```

**Expected Output:**
```json
[]
```

**Note:** An empty result confirms the VPC has been deleted.

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that no resources exist, as the VPC has been destroyed.

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

# Provision VPC
resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_vpc.this.cidr_block** | 10.0.0.0/16 | CIDR block for the VPC |
| **aws_vpc.this.tags.Name** | datacenter-vpc | Tag for VPC identification |

### Resource Properties
- **Region:** `us-east-1` as specified.
- **VPC State:** Deleted after `terraform destroy`.
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
aws_vpc.this
```

**Note:** The resource remains in the Terraform configuration but not in the state (destroyed).

### Step 2: Check Resource Details in Terraform State

```bash
# Show details of the VPC resource
terraform state show aws_vpc.this
```

**Expected Output:**
```
No resource found
```

**Note:** Since the VPC was destroyed, it no longer exists in the Terraform state.

### Step 3: Verify in AWS Console (Optional)

```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

**Expected Output:**
```json
{
    "Vpcs": []
}
```

---

## 🧪 Testing

### Verify VPC Deletion

1. Check the VPC existence:
```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

**Expected Output:**
```json
{
    "Vpcs": []
}
```

2. Attempt to re-provision (optional, for validation):
```bash
terraform apply -auto-approve
```

**Expected Output:** Re-creates the `datacenter-vpc`, confirming the configuration is intact.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision VPC
resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
terraform init
terraform destroy -target=aws_vpc.this -auto-approve
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_vpc.this

# Verify via AWS CLI
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: VPC Not Found**

- **Symptoms:** Error: "No VPC found with tag Name=datacenter-vpc"
- **Solution:** Verify the VPC exists or was already deleted
```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: ec2:DeleteVpc"
- **Solution:** Ensure AWS credentials have VPC permissions
```bash
# Required permissions:
# - ec2:DescribeVpcs
# - ec2:DeleteVpc
```

**Issue 3: VPC Has Dependencies**

- **Symptoms:** Error: "VPC has dependencies and cannot be deleted"
- **Solution:** Remove dependencies (e.g., subnets, route tables, internet gateways)
```bash
# List and delete subnets
aws ec2 describe-subnets --region us-east-1 --filters "Name=vpc-id,Values=vpc-0123456789abcdef0"
aws ec2 delete-subnet --region us-east-1 --subnet-id <subnet-id>

# List and delete internet gateways
aws ec2 describe-internet-gateways --region us-east-1 --filters "Name=attachment.vpc-id,Values=vpc-0123456789abcdef0"
aws ec2 detach-internet-gateway --region us-east-1 --internet-gateway-id <igw-id> --vpc-id vpc-0123456789abcdef0
aws ec2 delete-internet-gateway --region us-east-1 --internet-gateway-id <igw-id>

# Retry destroy
terraform destroy -target=aws_vpc.this -auto-approve
```

**Issue 4: Terraform Destroy Fails**

- **Symptoms:** Error: "Resource still in use or dependent"
- **Solution:** Ensure no dependencies exist and retry
```bash
terraform destroy -target=aws_vpc.this -auto-approve
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Ensured credentials have least-privilege access for VPC operations.
- **📊 Resource Naming:** Used clear names (`datacenter-vpc`) via tags.
- **🏷️ Minimal Configuration:** Retained the `main.tf` file unchanged for future use.
- **📍 Verifiability:** Ensured deletion is verifiable via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### VPC Cleanup Framework
- **State Management:** Ensure VPCs are deleted but configurations are preserved.
- **Monitoring:** Enable AWS CloudTrail to log VPC actions for auditing.
- **Resource Tagging:** Add tags for better resource tracking (optional enhancement).
- **Re-provisioning:** The retained `main.tf` allows quick re-provisioning with `terraform apply`.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name        = "datacenter-vpc"
    Team        = "nautilus-devops"
    Environment = "test"
    Purpose     = "migration-testing"
  }
}

output "vpc_id" {
  description = "ID of the VPC"
  value       = aws_vpc.this.id
}
```

### Security Strategy
- **Access Control:** Use IAM roles with specific VPC permissions.
- **Monitoring:** Log VPC actions with CloudTrail for audit trails.
- **Dependency Management:** Ensure VPCs are free of dependencies before deletion.
- **Validation:** Verify CIDR block and tags before re-provisioning.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **VPC Management:** Deleting VPCs with Terraform.
- **Configuration Retention:** Preserving Terraform code for future provisioning.
- **Resource Cleanup:** Safely removing cloud resources while maintaining configurations.
- **State Verification:** Confirming resource deletion using AWS CLI.

**Terraform Features Used:**
- `aws_vpc`: Managing the VPC.
- `terraform destroy`: Removing resources with a targeted approach.
- AWS provider integration for VPC services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **VPC Deleted**: Removed the `datacenter-vpc` in `us-east-1`.
- **Configuration Retained**: Kept the `main.tf` file unchanged for future provisioning.
- **Verification**: Confirmed the VPC is deleted via AWS CLI.
- **File Structure**: Used single `main.tf` file as specified.
- **Deployment**: Executed `terraform destroy` with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Cleanup Foundation:** The `datacenter-vpc` has been deleted, optimizing the Nautilus DevOps team’s AWS environment, with the configuration preserved for future use.

### 🔮 Resources Ready for:
- **Re-provisioning**: Use `terraform apply` to recreate the VPC.
- **Monitoring**: Audit VPC actions with CloudTrail.
- **Cost Management**: Removed unused VPC to streamline resources.