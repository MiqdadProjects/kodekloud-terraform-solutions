# 🌟 Task 31 - Delete AWS IAM Group Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is performing a cleanup process to remove temporary resources from their AWS account as part of a migration effort. An IAM group created for one-time use is no longer needed and must be deleted, while retaining the provisioning code for potential future use.

**Requirements:**
- Delete the IAM group named **`iamgroup_anita`** using Terraform.
- Retain the provisioning code in the `main.tf` file for potential re-provisioning.
- Ensure the group is deleted before task submission.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update or use the existing **`main.tf`** file (do not create a separate `.tf` file).

💡 **Note:** The IAM group `iamgroup_anita` must be deleted, but the Terraform configuration should remain intact for future use. Verification of the deletion is required. The current date and time is October 18, 2025, 05:42 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (IAM, region-agnostic)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- IAM group named `iamgroup_anita`
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **IAM Group Resource:** Manages the `iamgroup_anita` group.
- **Terraform Configuration:** Retains the `aws_iam_group` resource definition in `main.tf` for future provisioning.
- **Cleanup Process:** Uses `terraform destroy` with a targeted resource to delete the IAM group while preserving the configuration.

### 🎯 Implementation Strategy
1. Verify the existence of the `iamgroup_anita` IAM group using AWS CLI.
2. Use the provided `main.tf` configuration to manage the group.
3. Execute `terraform destroy` with a target to delete the IAM group.
4. Verify the group is deleted using AWS CLI.
5. Retain the `main.tf` file unchanged for future provisioning.

---

## 🚀 Implementation Steps

### Step 1: Verify Existing IAM Group

Confirm the `iamgroup_anita` group exists using AWS CLI.

```bash
# Check the IAM group details
aws iam get-group --group-name iamgroup_anita
```

**Example Output:**
```json
{
    "Group": {
        "Path": "/",
        "GroupName": "iamgroup_anita",
        "GroupId": "AGPA1234567890ABCDEF",
        "Arn": "arn:aws:iam::000000000000:group/iamgroup_anita",
        "CreateDate": "2025-10-18T17:00:00Z"
    },
    "Users": []
}
```

**Note:** Ensure AWS CLI is configured with credentials that have IAM permissions. If the group has users or policies attached, they must be removed before deletion.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Verify Main Terraform Configuration

The provided `main.tf` file contains the configuration for the `iamgroup_anita` group. Ensure it matches the following:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"  # IAM is global, but provider requires a region
}

# Provision IAM group
resource "aws_iam_group" "this" {
  name = "iamgroup_anita"
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (required for Terraform, though IAM is global).
- `aws_iam_group.this`: Defines the `iamgroup_anita` group.
- No additional attributes (e.g., policies or users) are specified, aligning with the provided solution.

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

### Step 6: Destroy the IAM Group

Execute the `terraform destroy` command with a target to delete the `iamgroup_anita` group.

```bash
terraform destroy -target=aws_iam_group.this -auto-approve
```

**Expected Output:**
```
aws_iam_group.this: Destroying... [id=iamgroup_anita]
aws_iam_group.this: Destruction complete after 1s

Destroy complete! Resources: 1 destroyed.
```

**Note:** The `main.tf` file remains unchanged, preserving the configuration for future use. The `-target` flag ensures only the specified resource is deleted.

---

### Step 7: Verify Group Deletion

Confirm the group is deleted using AWS CLI.

```bash
aws iam get-group --group-name iamgroup_anita
```

**Expected Output:**
```
An error occurred (NoSuchEntity) when calling the GetGroup operation: The group with name iamgroup_anita cannot be found.
```

**Note:** The `NoSuchEntity` error confirms the group has been deleted.

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that no resources exist, as the group has been destroyed.

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

# Provision IAM group
resource "aws_iam_group" "this" {
  name = "iamgroup_anita"
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_iam_group.this.name** | iamgroup_anita | Name of the IAM group |

### Resource Properties
- **Region:** IAM is global, but the provider requires a region (`us-east-1` used for consistency).
- **Group State:** Deleted after `terraform destroy`.
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
aws_iam_group.this
```

**Note:** The resource remains in the Terraform configuration but not in the state (destroyed).

### Step 2: Check Resource Details in Terraform State

```bash
# Show details of the group resource
terraform state show aws_iam_group.this
```

**Expected Output:**
```
No resource found
```

**Note:** Since the group was destroyed, it no longer exists in the Terraform state.

### Step 3: Verify in AWS Console (Optional)

```bash
aws iam get-group --group-name iamgroup_anita
```

**Expected Output:**
```
An error occurred (NoSuchEntity) when calling the GetGroup operation: The group with name iamgroup_anita cannot be found.
```

---

## 🧪 Testing

### Verify Group Deletion

1. Check the group existence:
```bash
aws iam get-group --group-name iamgroup_anita
```

**Expected Output:**
```
An error occurred (NoSuchEntity) when calling the GetGroup operation: The group with name iamgroup_anita cannot be found.
```

2. Attempt to re-provision (optional, for validation):
```bash
terraform apply -auto-approve
```

**Expected Output:** Re-creates the `iamgroup_anita` group, confirming the configuration is intact.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Provision IAM group
resource "aws_iam_group" "this" {
  name = "iamgroup_anita"
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
terraform init
terraform destroy -target=aws_iam_group.this -auto-approve
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_iam_group.this

# Verify via AWS CLI
aws iam get-group --group-name iamgroup_anita
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Group Not Found**

- **Symptoms:** Error: "NoSuchEntity: The group with name iamgroup_anita cannot be found"
- **Solution:** Verify the group exists or was already deleted
```bash
aws iam get-group --group-name iamgroup_anita
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: iam:DeleteGroup"
- **Solution:** Ensure AWS credentials have IAM permissions
```bash
# Required permissions:
# - iam:GetGroup
# - iam:DeleteGroup
```

**Issue 3: Group Has Dependencies**

- **Symptoms:** Error: "Group has users or policies attached"
- **Solution:** Remove users and policies from the group
```bash
# List and remove users
aws iam get-group --group-name iamgroup_anita
aws iam remove-user-from-group --group-name iamgroup_anita --user-name <user>

# List and detach policies
aws iam list-attached-group-policies --group-name iamgroup_anita
aws iam detach-group-policy --group-name iamgroup_anita --policy-arn <policy-arn>
```

**Issue 4: Terraform Destroy Fails**

- **Symptoms:** Error: "Resource still in use or dependent"
- **Solution:** Ensure no users or policies are attached and retry
```bash
terraform destroy -target=aws_iam_group.this -auto-approve
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Ensured credentials have least-privilege access for IAM operations.
- **📊 Resource Naming:** Used clear names (`iamgroup_anita`) for the group.
- **🏷️ Minimal Configuration:** Retained the `main.tf` file unchanged for future use.
- **📍 Verifiability:** Ensured deletion is verifiable via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### IAM Cleanup Framework
- **State Management:** Ensure groups are deleted but configurations are preserved.
- **Monitoring:** Enable AWS CloudTrail to log IAM actions for auditing.
- **Resource Tagging:** Add tags for better resource tracking (optional enhancement).
- **Re-provisioning:** The retained `main.tf` allows quick re-provisioning with `terraform apply`.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_group" "this" {
  name = "iamgroup_anita"

  tags = {
    Name        = "iamgroup_anita"
    Team        = "nautilus-devops"
    Environment = "test"
    Purpose     = "migration-testing"
  }
}

output "group_arn" {
  description = "ARN of the IAM group"
  value       = aws_iam_group.this.arn
}
```

### Security Strategy
- **Access Control:** Use IAM roles with specific permissions for group management.
- **Monitoring:** Log IAM actions with CloudTrail for audit trails.
- **Dependency Management:** Ensure groups are free of users and policies before deletion.
- **Validation:** Verify group configuration before re-provisioning.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **IAM Management:** Deleting IAM groups with Terraform.
- **Configuration Retention:** Preserving Terraform code for future provisioning.
- **Resource Cleanup:** Safely removing cloud resources while maintaining configurations.
- **State Verification:** Confirming resource deletion using AWS CLI.

**Terraform Features Used:**
- `aws_iam_group`: Managing the IAM group.
- `terraform destroy`: Removing resources with a targeted approach.
- AWS provider integration for IAM services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Group Deleted**: Removed the `iamgroup_anita` IAM group.
- **Configuration Retained**: Kept the `main.tf` file unchanged for future provisioning.
- **Verification**: Confirmed the group is deleted via AWS CLI.
- **File Structure**: Used single `main.tf` file as specified.
- **Deployment**: Executed `terraform destroy` with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Cleanup Foundation:** The `iamgroup_anita` IAM group has been deleted, optimizing the Nautilus DevOps team’s AWS environment, with the configuration preserved for future use.

### 🔮 Resources Ready for:
- **Re-provisioning**: Use `terraform apply` to recreate the group.
- **Monitoring**: Audit IAM actions with CloudTrail.
- **Cost Management**: Removed unused group to streamline IAM resources.