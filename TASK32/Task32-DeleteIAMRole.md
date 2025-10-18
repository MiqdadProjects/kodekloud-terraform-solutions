# 🌟 Task 32 - Delete AWS IAM Role Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is performing a cleanup process to remove temporary resources from their AWS account as part of a migration effort. An IAM role created for one-time use is no longer needed and must be deleted, while retaining the provisioning code for potential future use.

**Requirements:**
- Delete the IAM role named **`iamrole_james`** using Terraform.
- Retain the provisioning code in the `main.tf` file for potential re-provisioning.
- Ensure the role is deleted before task submission.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update or use the existing **`main.tf`** file (do not create a separate `.tf` file).

💡 **Note:** The IAM role `iamrole_james` must be deleted, but the Terraform configuration should remain intact for future use. Verification of the deletion is required. The current date and time is October 18, 2025, 05:47 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (IAM, region-agnostic)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- IAM role named `iamrole_james`
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **IAM Role Resource:** Manages the `iamrole_james` role with an assume role policy for EC2.
- **Terraform Configuration:** Retains the `aws_iam_role` resource definition in `main.tf` for future provisioning.
- **Cleanup Process:** Uses `terraform destroy` with a targeted resource to delete the IAM role while preserving the configuration.

### 🎯 Implementation Strategy
1. Verify the existence of the `iamrole_james` IAM role using AWS CLI.
2. Use the provided `main.tf` configuration to manage the role.
3. Execute `terraform destroy` with a target to delete the IAM role.
4. Verify the role is deleted using AWS CLI.
5. Retain the `main.tf` file unchanged for future provisioning.

---

## 🚀 Implementation Steps

### Step 1: Verify Existing IAM Role

Confirm the `iamrole_james` role exists using AWS CLI.

```bash
# Check the IAM role details
aws iam get-role --role-name iamrole_james
```

**Example Output:**
```json
{
    "Role": {
        "Path": "/",
        "RoleName": "iamrole_james",
        "RoleId": "AROA1234567890ABCDEF",
        "Arn": "arn:aws:iam::000000000000:role/iamrole_james",
        "CreateDate": "2025-10-18T17:00:00Z",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "ec2.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
    }
}
```

**Note:** Ensure AWS CLI is configured with credentials that have IAM permissions. If the role has attached policies, they must be detached before deletion.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Verify Main Terraform Configuration

The provided `main.tf` file contains the configuration for the `iamrole_james` role. Ensure it matches the following:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"  # IAM is global, but provider requires a region
}

# Provision IAM role
resource "aws_iam_role" "role" {
  name = "iamrole_james"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect    = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = "iamrole_james"
  }
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (required for Terraform, though IAM is global).
- `aws_iam_role.role`: Defines the `iamrole_james` role with an assume role policy allowing EC2 to assume it.
- `tags`: Applies the name `iamrole_james` for identification.
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

### Step 6: Destroy the IAM Role

Execute the `terraform destroy` command with a target to delete the `iamrole_james` role.

```bash
terraform destroy -target=aws_iam_role.role -auto-approve
```

**Expected Output:**
```
aws_iam_role.role: Destroying... [id=iamrole_james]
aws_iam_role.role: Destruction complete after 1s

Destroy complete! Resources: 1 destroyed.
```

**Note:** The `main.tf` file remains unchanged, preserving the configuration for future use. The `-target` flag ensures only the specified resource is deleted.

---

### Step 7: Verify Role Deletion

Confirm the role is deleted using AWS CLI.

```bash
aws iam get-role --role-name iamrole_james
```

**Expected Output:**
```
An error occurred (NoSuchEntity) when calling the GetRole operation: The role with name iamrole_james cannot be found.
```

**Note:** The `NoSuchEntity` error confirms the role has been deleted.

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that no resources exist, as the role has been destroyed.

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

# Provision IAM role
resource "aws_iam_role" "role" {
  name = "iamrole_james"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect    = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = "iamrole_james"
  }
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_iam_role.role.name** | iamrole_james | Name of the IAM role |
| **aws_iam_role.role.assume_role_policy** | JSON policy | Allows EC2 to assume the role |
| **aws_iam_role.role.tags.Name** | iamrole_james | Tag for role identification |

### Resource Properties
- **Region:** IAM is global, but the provider requires a region (`us-east-1` used for consistency).
- **Role State:** Deleted after `terraform destroy`.
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
aws_iam_role.role
```

**Note:** The resource remains in the Terraform configuration but not in the state (destroyed).

### Step 2: Check Resource Details in Terraform State

```bash
# Show details of the role resource
terraform state show aws_iam_role.role
```

**Expected Output:**
```
No resource found
```

**Note:** Since the role was destroyed, it no longer exists in the Terraform state.

### Step 3: Verify in AWS Console (Optional)

```bash
aws iam get-role --role-name iamrole_james
```

**Expected Output:**
```
An error occurred (NoSuchEntity) when calling the GetRole operation: The role with name iamrole_james cannot be found.
```

---

## 🧪 Testing

### Verify Role Deletion

1. Check the role existence:
```bash
aws iam get-role --role-name iamrole_james
```

**Expected Output:**
```
An error occurred (NoSuchEntity) when calling the GetRole operation: The role with name iamrole_james cannot be found.
```

2. Attempt to re-provision (optional, for validation):
```bash
terraform apply -auto-approve
```

**Expected Output:** Re-creates the `iamrole_james` role, confirming the configuration is intact.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision IAM role
resource "aws_iam_role" "role" {
  name = "iamrole_james"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect    = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = "iamrole_james"
  }
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
terraform init
terraform destroy -target=aws_iam_role.role -auto-approve
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_iam_role.role

# Verify via AWS CLI
aws iam get-role --role-name iamrole_james
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Role Not Found**

- **Symptoms:** Error: "NoSuchEntity: The role with name iamrole_james cannot be found"
- **Solution:** Verify the role exists or was already deleted
```bash
aws iam get-role --role-name iamrole_james
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: iam:DeleteRole"
- **Solution:** Ensure AWS credentials have IAM permissions
```bash
# Required permissions:
# - iam:GetRole
# - iam:DeleteRole
```

**Issue 3: Role Has Dependencies**

- **Symptoms:** Error: "Role has attached policies"
- **Solution:** Detach policies from the role
```bash
# List and detach policies
aws iam list-attached-role-policies --role-name iamrole_james
aws iam detach-role-policy --role-name iamrole_james --policy-arn <policy-arn>
```

**Issue 4: Terraform Destroy Fails**

- **Symptoms:** Error: "Resource still in use or dependent"
- **Solution:** Ensure no policies or resources are attached and retry
```bash
terraform destroy -target=aws_iam_role.role -auto-approve
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Ensured credentials have least-privilege access for IAM operations.
- **📊 Resource Naming:** Used clear names (`iamrole_james`) for the role.
- **🏷️ Minimal Configuration:** Retained the `main.tf` file unchanged for future use.
- **📍 Verifiability:** Ensured deletion is verifiable via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### IAM Cleanup Framework
- **State Management:** Ensure roles are deleted but configurations are preserved.
- **Monitoring:** Enable AWS CloudTrail to log IAM actions for auditing.
- **Resource Tagging:** Add tags for better resource tracking (optional enhancement).
- **Re-provisioning:** The retained `main.tf` allows quick re-provisioning with `terraform apply`.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_role" "role" {
  name = "iamrole_james"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect    = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name        = "iamrole_james"
    Team        = "nautilus-devops"
    Environment = "test"
    Purpose     = "migration-testing"
  }
}

output "role_arn" {
  description = "ARN of the IAM role"
  value       = aws_iam_role.role.arn
}
```

### Security Strategy
- **Access Control:** Use IAM roles with specific permissions for role management.
- **Monitoring:** Log IAM actions with CloudTrail for audit trails.
- **Dependency Management:** Ensure roles are free of policies before deletion.
- **Validation:** Verify role configuration before re-provisioning.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **IAM Management:** Deleting IAM roles with Terraform.
- **Configuration Retention:** Preserving Terraform code for future provisioning.
- **Resource Cleanup:** Safely removing cloud resources while maintaining configurations.
- **State Verification:** Confirming resource deletion using AWS CLI.

**Terraform Features Used:**
- `aws_iam_role`: Managing the IAM role.
- `terraform destroy`: Removing resources with a targeted approach.
- AWS provider integration for IAM services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Role Deleted**: Removed the `iamrole_james` IAM role.
- **Configuration Retained**: Kept the `main.tf` file unchanged for future provisioning.
- **Verification**: Confirmed the role is deleted via AWS CLI.
- **File Structure**: Used single `main.tf` file as specified.
- **Deployment**: Executed `terraform destroy` with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Cleanup Foundation:** The `iamrole_james` IAM role has been deleted, optimizing the Nautilus DevOps team’s AWS environment, with the configuration preserved for future use.

### 🔮 Resources Ready for:
- **Re-provisioning**: Use `terraform apply` to recreate the role.
- **Monitoring**: Audit IAM actions with CloudTrail.
- **Cost Management**: Removed unused role to streamline IAM resources.