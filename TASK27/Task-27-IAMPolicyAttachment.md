# 🌟 Task 27 - Attach AWS IAM Policy to IAM User Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is managing access control for their AWS infrastructure as part of a migration process. They need to attach an existing IAM policy to an existing IAM user to grant specific permissions.

**Requirements:**
- Attach the IAM policy named **`iampolicy_james`** to the IAM user named **`iamuser_james`** using Terraform.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update the **`main.tf`** file (do not create a separate `.tf` file).

👉 **Your task:** Update the Terraform configuration to attach the `iampolicy_james` IAM policy to the `iamuser_james` IAM user, ensuring the attachment is verifiable via Terraform and AWS CLI.

💡 **Note:** AWS IAM (Identity and Access Management) controls access to AWS resources. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable via Terraform and AWS CLI. The current date and time is September 30, 2025, 11:29 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (IAM, region-agnostic)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- IAM user named `iamuser_james`
- IAM policy named `iampolicy_james`
- IAM policy attachment to link the user and policy
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **IAM User Resource:** Manages the existing `iamuser_james` user.
- **IAM Policy Resource:** Manages the existing `iampolicy_james` policy.
- **IAM Policy Attachment Resource:** Attaches the policy to the user.
- **Security Framework:** Grants specific permissions (e.g., EC2 read actions) to the user.

### 🎯 Implementation Strategy
1. Identify the existing `iamuser_james` user and `iampolicy_james` policy using their ARNs or names.
2. Update the `main.tf` file to include the `aws_iam_user`, `aws_iam_policy`, and `aws_iam_user_policy_attachment` resources.
3. Import the existing user and policy into the Terraform state to manage them.
4. Deploy the updated configuration to attach the policy to the user.
5. Verify the policy attachment using Terraform state and AWS CLI.

---

## 🚀 Implementation Steps

### Step 1: Identify Existing Resources

Since the task states that the `iamuser_james` user and `iampolicy_james` policy already exist, retrieve their details using AWS CLI.

```bash
# Get the IAM user details if using aws cloud 
aws iam get-user --user-name iamuser_james
```

**Example Output:**
```json
{
    "User": {
        "Path": "/",
        "UserName": "iamuser_james",
        "UserId": "AIDAXYZ1234567890",
        "Arn": "arn:aws:iam::000000000000:user/iamuser_james",
        "CreateDate": "2025-09-30T23:29:00Z"
    }
}
```

```bash
# Get the IAM policy ARN
aws iam list-policies --query 'Policies[?PolicyName==`iampolicy_james`].Arn' --output text
```

**Example Output:**
```
arn:aws:iam::000000000000:policy/iampolicy_james
```

**Note:** The provided solution includes a sample policy granting EC2 read actions. Ensure the policy in your environment matches or adjust the `policy` attribute in `main.tf` if needed.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Update Main Terraform Configuration

Update the `main.tf` file with the configuration to manage and attach the existing resources:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"  # IAM is global, but provider requires a region
}

# Manage existing IAM user
resource "aws_iam_user" "user" {
  name = "iamuser_james"

  tags = {
    Name = "iamuser_james"
  }
}

# Manage existing IAM policy
resource "aws_iam_policy" "policy" {
  name        = "iampolicy_james"
  description = "IAM policy allowing EC2 read actions for james"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Describe*"]
        Resource = "*"
      }
    ]
  })
}

# Attach IAM policy to IAM user
resource "aws_iam_user_policy_attachment" "james_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (IAM is global, but a region is required for the provider).
- `aws_iam_user`: Manages the existing `iamuser_james` user.
- `aws_iam_policy`: Manages the existing `iampolicy_james` policy with a sample policy allowing EC2 read actions (adjusted to use `ec2:Describe*` for precision).
- `aws_iam_user_policy_attachment`: Attaches the policy to the user using their respective names and ARNs.
- `tags`: Applies the required name (`iamuser_james`) to the user via tags.
- `policy`: Defines the policy as provided, allowing EC2 read actions (e.g., `ec2:Describe*`).

**Note:** The policy uses `ec2:Describe*` instead of `ec2:Read*` (from the provided solution) because `Read*` is not a valid IAM action prefix. Verify the actual policy definition using:
```bash
aws iam get-policy-version --policy-arn arn:aws:iam::000000000000:policy/iampolicy_james --version-id v1
```

---

### Step 4: Import Existing Resources

Since the `iamuser_james` user and `iampolicy_james` policy already exist, import them into the Terraform state to manage them.

```bash
# Import the IAM user
terraform import aws_iam_user.user iamuser_james
```

```bash
# Import the IAM policy
terraform import aws_iam_policy.policy arn:aws:iam::000000000000:policy/iampolicy_james
```

**Note:** Replace `000000000000` with your actual AWS account ID. Use the ARN from Step 1 for the policy import.

---

### Step 5: Initialize Terraform (if not already initialized)

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

### Step 6: Format and Validate Configuration

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

### Step 7: Plan and Apply Configuration

```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply -auto-approve
```

**Expected Output:**
```
Terraform will perform the following actions:

  # aws_iam_user_policy_attachment.james_attach will be created
  + resource "aws_iam_user_policy_attachment" "james_attach" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::000000000000:policy/iampolicy_james"
      + user       = "iamuser_james"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_iam_user.user: Refreshing state... [id=iamuser_james]
aws_iam_policy.policy: Refreshing state... [id=arn:aws:iam::000000000000:policy/iampolicy_james]
aws_iam_user_policy_attachment.james_attach: Refreshing state... [id=iamuser_james-arn:aws:iam::000000000000:policy/iampolicy_james]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

### Step 9: Verify IAM Policy Attachment

```bash
# Check the IAM user policy attachment using AWS CLI
aws iam list-attached-user-policies --user-name iamuser_james
```

**Expected Output:**
```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "iampolicy_james",
            "PolicyArn": "arn:aws:iam::000000000000:policy/iampolicy_james"
        }
    ]
}
```

```bash
# Verify the policy details
aws iam get-policy --policy-arn arn:aws:iam::000000000000:policy/iampolicy_james
```

**Expected Output (partial):**
```json
{
    "Policy": {
        "PolicyName": "iampolicy_james",
        "PolicyId": "ANPAXYZ1234567890",
        "Arn": "arn:aws:iam::000000000000:policy/iampolicy_james",
        "Description": "IAM policy allowing EC2 read actions for james"
    }
}
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing IAM user
resource "aws_iam_user" "user" {
  name = "iamuser_james"

  tags = {
    Name = "iamuser_james"
  }
}

# Manage existing IAM policy
resource "aws_iam_policy" "policy" {
  name        = "iampolicy_james"
  description = "IAM policy allowing EC2 read actions for james"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Describe*"]
        Resource = "*"
      }
    ]
  })
}

# Attach IAM policy to IAM user
resource "aws_iam_user_policy_attachment" "james_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_iam_user.name** | iamuser_james | Name of the IAM user |
| **aws_iam_user.tags.Name** | iamuser_james | Tag for user identification |
| **aws_iam_policy.name** | iampolicy_james | Name of the IAM policy |
| **aws_iam_policy.description** | IAM policy allowing EC2 read actions for james | Description of the policy |
| **aws_iam_policy.policy** | JSON policy | Allows EC2 `Describe*` actions |
| **aws_iam_user_policy_attachment.user** | iamuser_james | User to attach the policy to |
| **aws_iam_user_policy_attachment.policy_arn** | (Dynamic) | ARN of the `iampolicy_james` policy |

### Resource Properties
- **Region:** IAM is global, but the provider requires a region (`us-east-1` used for consistency).
- **Policy Scope:** The policy grants read-only access to EC2 resources (e.g., `ec2:Describe*`).
- **Attachment:** Links the user and policy, granting the specified permissions.

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_iam_user.user
aws_iam_policy.policy
aws_iam_user_policy_attachment.james_attach
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed attachment information
terraform state show aws_iam_user_policy_attachment.james_attach
```

**Expected Output:**
```
# aws_iam_user_policy_attachment.james_attach:
resource "aws_iam_user_policy_attachment" "james_attach" {
    id         = "iamuser_james-arn:aws:iam::000000000000:policy/iampolicy_james"
    policy_arn = "arn:aws:iam::000000000000:policy/iampolicy_james"
    user       = "iamuser_james"
}
```

### Step 3: Verify in AWS Console (Optional)

```bash
# Check attached policies for the user
aws iam list-attached-user-policies --user-name iamuser_james
```

**Expected JSON Output:**
```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "iampolicy_james",
            "PolicyArn": "arn:aws:iam::000000000000:policy/iampolicy_james"
        }
    ]
}
```

```bash
# Verify policy permissions
aws iam get-policy-version --policy-arn arn:aws:iam::000000000000:policy/iampolicy_james --version-id v1
```

**Expected JSON Output (partial):**
```json
{
    "PolicyVersion": {
        "Document": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": ["ec2:Describe*"],
                    "Resource": "*"
                }
            ]
        },
        "VersionId": "v1"
    }
}
```

---

## 🧪 Testing

### Verify Policy Attachment

```bash
# Verify the policy is attached to the user
aws iam list-attached-user-policies --user-name iamuser_james
```

**Expected Output:**
```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "iampolicy_james",
            "PolicyArn": "arn:aws:iam::000000000000:policy/iampolicy_james"
        }
    ]
}
```

### Test Policy Permissions (Optional)

```bash
# Assume the user's credentials and test EC2 Describe actions
aws sts assume-role --role-arn arn:aws:iam::000000000000:role/test-role --role-session-name test-session
# Export temporary credentials and test
aws ec2 describe-instances
```

**Expected Output:** List of EC2 instances, indicating the `ec2:Describe*` permissions are effective.

**Note:** This requires the user to have appropriate credentials or an assumed role. Adjust the test based on your authentication setup.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing IAM user
resource "aws_iam_user" "user" {
  name = "iamuser_james"

  tags = {
    Name = "iamuser_james"
  }
}

# Manage existing IAM policy
resource "aws_iam_policy" "policy" {
  name        = "iampolicy_james"
  description = "IAM policy allowing EC2 read actions for james"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Describe*"]
        Resource = "*"
      }
    ]
  })
}

# Attach IAM policy to IAM user
resource "aws_iam_user_policy_attachment" "james_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
# Import existing resources
terraform import aws_iam_user.user iamuser_james
terraform import aws_iam_policy.policy arn:aws:iam::000000000000:policy/iampolicy_james
terraform init
terraform apply -auto-approve
terraform plan
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_iam_user_policy_attachment.james_attach

# Verify via AWS CLI
aws iam list-attached-user-policies --user-name iamuser_james
aws iam get-policy-version --policy-arn arn:aws:iam::000000000000:policy/iampolicy_james --version-id v1
```

### Optional: Enhanced Configuration

```hcl
# Enhanced configuration with additional settings
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_user" "user" {
  name = "iamuser_james"

  tags = {
    Name        = "iamuser_james"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "ec2-management"
  }
}

resource "aws_iam_policy" "policy" {
  name        = "iampolicy_james"
  description = "IAM policy allowing EC2 read actions for james"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Describe*"]
        Resource = "*"
      }
    ]
  })
}

resource "aws_iam_user_policy_attachment" "james_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}

output "user_arn" {
  description = "ARN of the IAM user"
  value       = aws_iam_user.user.arn
}

output "policy_arn" {
  description = "ARN of the IAM policy"
  value       = aws_iam_policy.policy.arn
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Resource Already Exists**

- **Symptoms:** Error: "User: iamuser_james is already exists" or "Policy: iampolicy_james already exists"
- **Solution:** Import the existing user and policy
```bash
terraform import aws_iam_user.user iamuser_james
terraform import aws_iam_policy.policy arn:aws:iam::000000000000:policy/iampolicy_james
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: iam:AttachUserPolicy"
- **Solution:** Ensure AWS credentials have IAM permissions
```bash
# Required permissions:
# - iam:AttachUserPolicy
# - iam:GetUser
# - iam:GetPolicy
# - iam:GetPolicyVersion
# - iam:ListAttachedUserPolicies
```

**Issue 3: Invalid Policy Document**

- **Symptoms:** Error: "MalformedPolicyDocument: Invalid policy document"
- **Solution:** Validate the policy JSON
```bash
# Validate JSON syntax
echo '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":["ec2:Describe*"],"Resource":"*"}]}' | jq .
```

**Issue 4: Terraform Plan Shows Changes**

- **Symptoms:** `terraform plan` indicates differences after `apply`
- **Solution:** Verify `main.tf` matches the deployed configuration and reapply
```bash
terraform fmt
terraform validate
terraform apply -auto-approve
terraform plan
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Attached a specific IAM policy to grant least-privilege access.
- **📊 Resource Naming:** Clear names (`iamuser_james`, `iampolicy_james`) via tags and attributes.
- **🏷️ Minimal Configuration:** Focused on policy attachment, preserving existing settings.
- **🔄 Resource Import:** Imported existing resources to manage them with Terraform.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### IAM Security Framework
- **Least Privilege:** Restrict the policy to specific EC2 resources (e.g., specific ARNs) instead of `*`.
- **Monitoring:** Enable AWS CloudTrail to log IAM actions.
- **Tagging:** Add tags for cost allocation and resource management.
- **Access Keys:** Avoid creating access keys for the user unless necessary; use IAM roles for applications.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_user" "user" {
  name = "iamuser_james"

  tags = {
    Name        = "iamuser_james"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "ec2-management"
  }
}

resource "aws_iam_policy" "policy" {
  name        = "iampolicy_james"
  description = "IAM policy allowing EC2 read actions for james"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = ["ec2:Describe*"]
        Resource = "*"
      }
    ]
  })
}

resource "aws_iam_user_policy_attachment" "james_attach" {
  user       = aws_iam_user.user.name
  policy_arn = aws_iam_policy.policy.arn
}
```

### Security Strategy
- **Policy Scope:** Narrow the policy to specific resources (e.g., `arn:aws:ec2:us-east-1:000000000000:instance/*`).
- **Monitoring:** Set up CloudTrail to audit IAM actions.
- **Access Management:** Use groups for shared permissions instead of direct policy attachments.
- **Cost Optimization:** Monitor IAM usage to avoid unnecessary costs.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **IAM Fundamentals:** Understanding IAM users, policies, and attachments.
- **Policy Management:** Defining and attaching IAM policies with Terraform.
- **Resource Import:** Importing existing IAM resources into Terraform state.
- **Dependency Management:** Ensuring proper resource dependencies in Terraform.

**Terraform Features Used:**
- `aws_iam_user`: Managing existing IAM user.
- `aws_iam_policy`: Managing existing IAM policy.
- `aws_iam_user_policy_attachment`: Attaching policy to user.
- AWS provider integration for IAM services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Policy Attachment**: Attached `iampolicy_james` to `iamuser_james` as required.
- **File Structure**: Updated single `main.tf` file as specified.
- **Verification**: Confirmed attachment via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Security Foundation:** The IAM policy `iampolicy_james` is attached to the `iamuser_james` user, granting EC2 read permissions and enabling secure access control for the Nautilus DevOps team’s application.

### 🔮 Resources Ready for:
- **Access Control**: Grant EC2 read access to the user.
- **Monitoring**: Audit IAM actions with CloudTrail.
- **Security**: Enhance with scoped policies and group-based permissions.
- **Integration**: Use the user for programmatic access to EC2 resources.