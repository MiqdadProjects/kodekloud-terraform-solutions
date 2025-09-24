# 🌟 Task 16 - Create IAM Policy Using Terraform

## 📌 Task Description

When establishing infrastructure on the AWS cloud, **Identity and Access Management (IAM)** is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The **Nautilus DevOps team** is currently in the process of configuring these resources and has outlined the following requirements.

IAM policies define permissions that can be attached to users, groups, or roles to control access to AWS resources. The team requires a policy that grants read-only access to the EC2 console for viewing instances, AMIs, and snapshots.

**Requirements:**
- Create an IAM policy named **`iampolicy_yousuf`** in the **us-east-1** region using Terraform
- The policy must allow read-only access to the EC2 console (view all instances, AMIs, and snapshots)
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)
- Assume the AWS provider is already configured in `providers.tf` for the us-east-1 region

👉 **Your task:** Create an IAM policy using Terraform to provide read-only access to the EC2 console as part of the Nautilus DevOps team's identity and access management strategy.

�წ

System: 💡 **Note:** IAM policies define specific permissions for AWS resources, ensuring secure and controlled access.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- IAM Policy for EC2 read-only access
- Permissions for viewing instances, AMIs, and snapshots
- Foundation for attaching to users, groups, or roles

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **IAM Policy Resource:** Defines permissions for EC2 console read-only access
- **Access Management:** Grants specific read-only actions for EC2 resources
- **Security Framework:** Implements least-privilege access for EC2 console

### 🎯 Implementation Strategy
1. Create IAM policy with specified name (`iampolicy_yousuf`)
2. Define policy document to allow read-only access to EC2 instances, AMIs, and snapshots
3. Deploy in the us-east-1 region (provider configured in `providers.tf`)
4. Verify policy creation and accessibility

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

# Create IAM policy for EC2 read-only access
resource "aws_iam_policy" "iampolicy_yousuf" {
  name        = "iampolicy_yousuf"
  description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots"
        ]
        Resource = "*"
      }
    ]
  })
}
```

**Configuration Breakdown:**
- `aws_iam_policy` - Creates an IAM policy resource
- `name = "iampolicy_yousuf"` - Sets the required policy name
- `description` - Describes the policy's purpose
- `policy` - JSON policy document granting read-only EC2 permissions
- `Action` - Includes `ec2:DescribeInstances`, `ec2:DescribeImages`, and `ec2:DescribeSnapshots` for read-only access
- `Resource = "*"` - Applies permissions to all relevant EC2 resources

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

  # aws_iam_policy.iampolicy_yousuf will be created
  + resource "aws_iam_policy" "iampolicy_yousuf" {
      + arn         = (known after apply)
      + description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"
      + id          = (known after apply)
      + name        = "iampolicy_yousuf"
      + path        = "/"
      + policy      = jsonencode(
            {
              Statement = [
                {
                  Action   = [
                    "ec2:DescribeInstances",
                    "ec2:DescribeImages",
                    "ec2:DescribeSnapshots",
                  ]
                  Effect   = "Allow"
                  Resource = "*"
                },
              ]
              Version   = "2012-10-17"
            }
        )
      + policy_id   = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create IAM policy for EC2 read-only access
resource "aws_iam_policy" "iampolicy_yousuf" {
  name        = "iampolicy_yousuf"
  description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots"
        ]
        Resource = "*"
      }
    ]
  })
}
```

### IAM Policy Attributes
| Attribute | Default Value | Description |
|-----------|---------------|-------------|
| **name** | iampolicy_yousuf | The name of the IAM policy |
| **description** | Read-only access to EC2 console | Description of the policy's purpose |
| **path** | / | The path for the policy (default root path) |
| **policy** | (defined JSON) | JSON document defining permissions |
| **arn** | (assigned by AWS) | Amazon Resource Name for the policy |

### IAM Policy Default Properties
- **Path:** `/` (root path by default)
- **Permissions:** Grants `ec2:DescribeInstances`, `ec2:DescribeImages`, `ec2:DescribeSnapshots`
- **Attachments:** Not attached to any users, groups, or roles initially
- **Scope:** Applies to all EC2 resources (`Resource = "*"`)
- **Version:** Uses IAM policy version `2012-10-17`

---

## ✅ Verification Steps

### Step 1: Verify IAM Policy Creation
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_iam_policy.iampolicy_yousuf
```

### Step 2: Check Policy Details in Terraform State
```bash
# Show detailed policy information from Terraform state
terraform state show aws_iam_policy.iampolicy_yousuf
```

**Expected Output:**
```
# aws_iam_policy.iampolicy_yousuf:
resource "aws_iam_policy" "iampolicy_yousuf" {
    arn         = "arn:aws:iam::123456789012:policy/iampolicy_yousuf"
    description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"
    id          = "arn:aws:iam::123456789012:policy/iampolicy_yousuf"
    name        = "iampolicy_yousuf"
    path        = "/"
    policy      = jsonencode(
        {
            Statement = [
                {
                    Action   = [
                        "ec2:DescribeInstances",
                        "ec2:DescribeImages",
                        "ec2:DescribeSnapshots",
                    ]
                    Effect   = "Allow"
                    Resource = "*"
                },
            ]
            Version   = "2012-10-17"
        }
    )
    policy_id   = "ANPAJAVEDPOLICYEXAMPLE"
}
```

### Step 3: Verify Policy in AWS Console (Optional)
```bash
# List IAM policies using AWS CLI
aws iam list-policies --query 'Policies[?PolicyName==`iampolicy_yousuf`]'
```

**Expected JSON Output:**
```json
[
    {
        "PolicyName": "iampolicy_yousuf",
        "PolicyId": "ANPAJAVEDPOLICYEXAMPLE",
        "Arn": "arn:aws:iam::123456789012:policy/iampolicy_yousuf",
        "Path": "/",
        "CreateDate": "2025-XX-XXTXX:XX:XX+00:00"
    }
]
```

---

## 🧪 Testing

### Verify Policy Creation
```bash
# Show complete policy details from Terraform state
terraform state show aws_iam_policy.iampolicy_yousuf
```

### Test Policy Existence via AWS CLI
```bash
# Get specific policy details
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/iampolicy_yousuf
```

### Check Policy Document
```bash
# Retrieve the policy document
aws iam get-policy-version --policy-arn arn:aws:iam::123456789012:policy/iampolicy_yousuf --version-id v1
```

**Expected JSON Output:**
```json
{
    "PolicyVersion": {
        "Document": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "ec2:DescribeInstances",
                        "ec2:DescribeImages",
                        "ec2:DescribeSnapshots"
                    ],
                    "Resource": "*"
                }
            ]
        },
        "VersionId": "v1",
        "IsDefaultVersion": true,
        "CreateDate": "2025-XX-XXTXX:XX:XX+00:00"
    }
}
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create IAM policy for EC2 read-only access
resource "aws_iam_policy" "iampolicy_yousuf" {
  name        = "iampolicy_yousuf"
  description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots"
        ]
        Resource = "*"
      }
    ]
  })
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
# Verify in Terraform
terraform state list

# Verify via AWS CLI
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/iampolicy_yousuf
```

### Optional: Enhanced Configuration
```hcl
# Enhanced IAM policy with additional attributes
resource "aws_iam_policy" "iampolicy_yousuf" {
  name        = "iampolicy_yousuf"
  path        = "/policies/"
  description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots",
          "ec2:DescribeRegions",
          "ec2:DescribeAvailabilityZones"
        ]
        Resource = "*"
      }
    ]
  })

  tags = {
    Name        = "iampolicy_yousuf"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "ec2-read-only"
  }
}

# Optional: Attach policy to a group (requires group to exist)
resource "aws_iam_group_policy_attachment" "yousuf_policy_attachment" {
  group      = "existing-group-name"
  policy_arn = aws_iam_policy.iampolicy_yousuf.arn
}
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient policy information
output "policy_name" {
  description = "Name of the created IAM policy"
  value       = aws_iam_policy.iampolicy_yousuf.name
}

output "policy_arn" {
  description = "ARN of the created IAM policy"
  value       = aws_iam_policy.iampolicy_yousuf.arn
}

output "policy_id" {
  description = "ID of the created IAM policy"
  value       = aws_iam_policy.iampolicy_yousuf.policy_id
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Policy already exists**
- **Symptoms:** Error: "EntityAlreadyExists"
- **Solution:** Import existing policy or use different name
```bash
# Import existing policy into Terraform state
terraform import aws_iam_policy.iampolicy_yousuf arn:aws:iam::123456789012:policy/iampolicy_yousuf

# Or use a different policy name
```

**Issue 2: Insufficient permissions to create IAM policies**
- **Symptoms:** Error: "AccessDenied" or "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have IAM policy creation permissions
```bash
# Required permissions:
# - iam:CreatePolicy
# - iam:GetPolicy
# - iam:ListPolicies
# - iam:TagPolicy (if using tags)
```

**Issue 3: Invalid policy document format**
- **Symptoms:** Error: "MalformedPolicyDocument"
- **Solution:** Ensure JSON syntax is correct and adheres to IAM policy structure
```bash
# Ensure:
# - Valid JSON syntax
# - Correct Version ("2012-10-17")
# - Valid Action names and Resource format
```

**Issue 4: Region-specific issues**
- **Symptoms:** Error accessing resources in us-east-1
- **Solution:** Ensure provider is configured for us-east-1 in `providers.tf`
```hcl
provider "aws" {
  region = "us-east-1"
}
```

---

## 💡 Best Practices Applied

- **🔐 Security Foundation:** Creating policy with specific, read-only permissions
- **📊 Resource Management:** Simple, focused policy creation following AWS standards
- **🏷️ Naming Conventions:** Clear, descriptive policy name matching requirements
- **🔄 Least Privilege:** Policy grants only necessary EC2 read-only permissions
- **📍 Access Strategy:** Foundation for attaching to users, groups, or roles

---

## 🚀 Production Considerations

### IAM Policy Security Framework
- **Policy Attachment:** Attach to appropriate users, groups, or roles
- **Policy Versioning:** Use versioning for policy updates
- **Access Review:** Regularly audit policy attachments and permissions
- **Monitoring:** Enable CloudTrail for policy usage logging
- **Documentation:** Maintain clear documentation of policy purpose

### Enhanced Policy Configuration
```hcl
# Production-ready IAM policy with comprehensive settings
resource "aws_iam_policy" "iampolicy_yousuf" {
  name        = "iampolicy_yousuf"
  path        = "/policies/"
  description = "Read-only access to EC2 console (instances, AMIs, and snapshots)"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots",
          "ec2:DescribeRegions",
          "ec2:DescribeAvailabilityZones",
          "ec2:DescribeSecurityGroups",
          "ec2:DescribeSubnets",
          "ec2:DescribeVpcs"
        ]
        Resource = "*"
      }
    ]
  })

  tags = {
    Name        = "iampolicy_yousuf"
    Department  = "DevOps"
    Team        = "Nautilus"
    Environment = "Production"
    CostCenter  = "Engineering"
  }
}
```

### Access Management Strategy
- **Group Attachments:** Attach policy to groups for scalable access control
- **Role-Based Access:** Use with IAM roles for temporary credentials
- **Policy Scoping:** Consider scoping resources to specific regions or accounts
- **Regular Audits:** Review policy permissions and attachments periodically
- **Least Privilege:** Expand permissions only as needed

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **IAM Policy Creation:** Understanding AWS permission definitions
- **Access Control:** Building blocks for secure resource access
- **Security Principles:** Implementing least-privilege access for EC2

**Terraform Features Used:**
- `aws_iam_policy` - IAM policy resource creation
- JSON policy document definition
- IAM service integration

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Policy Name** - Created "iampolicy_yousuf" as required
- **Region** - Deployed in us-east-1 (provider configured in `providers.tf`)
- **Permissions** - Granted read-only access to EC2 console (instances, AMIs, snapshots)
- **Resource Management** - Proper Terraform state management
- **Verification** - Policy existence confirmed through multiple methods
- **File Structure** - Used single `main.tf` file as specified

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Access Foundation:** The IAM policy "iampolicy_yousuf" is now created in the us-east-1 region and ready for attachment to users, groups, or roles as part of the Nautilus DevOps team's identity and access management strategy.

### 🔮 Policy Ready for:
- **Attachment to Users/Groups/Roles:** Assign to appropriate IAM entities
- **Access Testing:** Verify functionality in EC2 console
- **Policy Expansion:** Add additional read-only permissions as needed
- **Security Auditing:** Monitor usage and ensure compliance
- **Permission Management:** Implement role-based access controls