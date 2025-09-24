# 🌟 Task 15 - Create IAM Group Using Terraform

## 📌 Task Description

The **Javed DevOps team** is actively migrating services to the AWS cloud, adopting a modular approach to ensure better control, risk mitigation, and resource optimization. As part of their identity and access management strategy, they require the creation of an IAM group to organize user permissions effectively.

IAM groups are fundamental for managing permissions centrally, allowing multiple users to share the same access controls within the AWS environment.

**Requirements:**
- Create an IAM group named **`iamgroup_javed`** using Terraform
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create an IAM group using Terraform as the foundation for centralized permission management in the AWS environment.

💡 **Note:** IAM groups provide a mechanism to assign permissions to multiple users efficiently.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (global service)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- IAM Group for centralized permission management
- Foundation for user group assignments
- Framework for access control policies

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **IAM Group Resource:** Creates a group for managing user permissions
- **Access Management:** Prepares group for policy attachments
- **Security Framework:** Foundation for implementing role-based access control

### 🎯 Implementation Strategy
1. Create IAM group with specified name (`iamgroup_javed`)
2. Establish group identity in AWS IAM service
3. Prepare foundation for user assignments and policy attachments
4. Verify group creation and accessibility

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

# Create IAM group for centralized permission management
resource "aws_iam_group" "iamgroup_javed" {
  name = "iamgroup_javed"
}
```

**Configuration Breakdown:**
- `aws_iam_group` - Creates an IAM group resource
- `name = "iamgroup_javed"` - Sets the required group name as specified

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

  # aws_iam_group.iamgroup_javed will be created
  + resource "aws_iam_group" "iamgroup_javed" {
      + arn       = (known after apply)
      + id        = (known after apply)
      + name      = "iamgroup_javed"
      + path      = "/"
      + unique_id = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create IAM group for centralized permission management
resource "aws_iam_group" "iamgroup_javed" {
  name = "iamgroup_javed"                  # Required group name
}
```

### IAM Group Attributes
| Attribute | Default Value | Description |
|-----------|---------------|-------------|
| **name** | iamgroup_javed | The name of the IAM group |
| **path** | / | The path for the group (default root path) |
| **arn** | (assigned by AWS) | Amazon Resource Name for the group |
| **unique_id** | (assigned by AWS) | Unique identifier for the group |

### IAM Group Default Properties
- **Path:** `/` (root path by default)
- **Permissions:** No policies attached by default (follows least privilege)
- **Members:** No users assigned initially
- **Policies:** No managed or inline policies attached initially

---

## ✅ Verification Steps

### Step 1: Verify IAM Group Creation
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_iam_group.iamgroup_javed
```

### Step 2: Check Group Details in Terraform State
```bash
# Show detailed group information from Terraform state
terraform state show aws_iam_group.iamgroup_javed
```

**Expected Output:**
```
# aws_iam_group.iamgroup_javed:
resource "aws_iam_group" "iamgroup_javed" {
    arn       = "arn:aws:iam::123456789012:group/iamgroup_javed"
    id        = "iamgroup_javed"
    name      = "iamgroup_javed"
    path      = "/"
    unique_id = "AGPAJAVEDGROUPEXAMPLE"
}
```

### Step 3: Verify Group in AWS Console (Optional)
```bash
# List IAM groups using AWS CLI
aws iam list-groups --query 'Groups[?GroupName==`iamgroup_javed`]'
```

**Expected JSON Output:**
```json
[
    {
        "GroupName": "iamgroup_javed",
        "Path": "/",
        "CreateDate": "2025-XX-XXTXX:XX:XX+00:00",
        "GroupId": "AGPAJAVEDGROUPEXAMPLE",
        "Arn": "arn:aws:iam::123456789012:group/iamgroup_javed"
    }
]
```

---

## 🧪 Testing

### Verify Group Creation
```bash
# Show complete group details from Terraform state
terraform state show aws_iam_group.iamgroup_javed
```

