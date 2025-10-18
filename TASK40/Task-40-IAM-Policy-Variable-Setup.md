````markdown
# 🌟 Task 40 - IAM Policy Variable Setup Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is automating IAM policy creation using Terraform to enhance security and access management. This task focuses on implementing parameterized IAM policy configurations that enable consistent, reusable policy provisioning across the organization.

Using Terraform variables for IAM policy creation allows teams to maintain consistent naming conventions, simplify policy provisioning workflows, and enable easy deployment across multiple environments with standardized permission sets.

**Requirements:**
- Create an AWS IAM policy using Terraform with variable-based configuration
- The IAM policy name **`iampolicy_ammar`** should be stored in a variable named **`KKE_iampolicy`**
- Configuration values should be stored in a **`variables.tf`** file
- The Terraform script should be structured with **`main.tf`** referencing **`variables.tf`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create separate files: `main.tf` and `variables.tf`
- Include IAM policy document with EC2 and S3 permissions

👉 **Your task:** Implement parameterized IAM policy configuration using Terraform variables for enhanced access management automation.

💡 **Note:** Using variables enables IAM policy configurations to be reused across different organizational units and environments with consistent permission sets.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (global service)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- IAM Policy with variable-based configuration
- Parameterized permission statements
- Reusable policy provisioning template
- EC2 and S3 service permissions

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Variables File:** Centralized configuration storage for IAM policy naming
- **IAM Policy Resource:** References variable for dynamic policy creation
- **Policy Document:** JSON-encoded permission statements
- **Service Permissions:** EC2 describe and S3 list operations
- **Infrastructure as Code:** Parameterized, reusable configuration pattern
- **Tagging Strategy:** Uses variable for consistent resource identification

### 🎯 Implementation Strategy
1. Define variable `KKE_iampolicy` in `variables.tf` with default value
2. Create IAM Policy in `main.tf` with policy document
3. Configure permission statements for EC2 and S3 services
4. Implement proper tagging using the variable for consistency
5. Structure files for maximum reusability and maintainability

---

## 🚀 Implementation Steps

### Step 1: Navigate to Working Directory
```bash
cd /home/bob/terraform
```

**Purpose:** Ensure we're working in the correct Terraform directory as specified in the requirements.

---

### Step 2: Create Variables File

Create the `/home/bob/terraform/variables.tf` file:
```hcl
# variables.tf

# Define variable for IAM policy name
variable "KKE_iampolicy" {
  description = "The name of the IAM policy to create"
  type        = string
  default     = "iampolicy_ammar"
}
```

**Configuration Breakdown:**
- `variable "KKE_iampolicy"` - Defines variable with name matching requirements
- `description` - Explains the variable's purpose for documentation
- `type = string` - Specifies variable data type
- `default = "iampolicy_ammar"` - Provides default value as specified in requirements

---

### Step 3: Create Main Configuration File

Create the `/home/bob/terraform/main.tf` file:
```hcl
# main.tf

# Create AWS IAM Policy with variable reference
resource "aws_iam_policy" "this" {
  name        = var.KKE_iampolicy
  description = "Custom IAM policy created for the Nautilus DevOps team"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:Describe*",
          "s3:ListAllMyBuckets"
        ]
        Effect   = "Allow"
        Resource = "*"
      }
    ]
  })
  
  tags = {
    Name = var.KKE_iampolicy
  }
}
```

**Configuration Breakdown:**
- `aws_iam_policy` - Creates IAM Policy resource
- `name = var.KKE_iampolicy` - References variable for dynamic policy naming
- `description` - Explains the policy purpose
- `policy = jsonencode()` - Encodes policy document as JSON
- `Version = "2012-10-17"` - IAM policy language version
- `Statement` - Policy statement array
- `Action` - EC2 describe and S3 list operations
- `Effect = "Allow"` - Permits the specified actions
- `Resource = "*"` - Applies to all resources
- `tags` - Assigns tags with variable reference for policy identification

---

### Step 4: Initialize Terraform
```bash
terraform init
```

**Purpose:** Initialize the Terraform working directory and download required providers.

**Expected Output:**
````
Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v5.20.0...
- Using previously-installed hashicorp/aws v5.20.0

Terraform has been successfully initialized!
````

---

### Step 5: Format and Validate Configuration
```bash
# Format all Terraform files
terraform fmt

# Validate configuration
terraform validate
```

**Purpose:** Ensure code formatting consistency and validate syntax correctness.

**Expected Output:**
````
Success! The configuration is valid.
````

---

### Step 6: Plan and Apply Configuration
```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply -auto-approve
```

**Expected Output:**
````
Terraform will perform the following actions:

  # aws_iam_policy.this will be created
  + resource "aws_iam_policy" "this" {
      + arn                 = (known after apply)
      + attachment_count    = (known after apply)
      + create_date         = (known after apply)
      + description         = "Custom IAM policy created for the Nautilus DevOps team"
      + id                  = (known after apply)
      + name                = "iampolicy_ammar"
      + path                = "/"
      + policy              = jsonencode(
          {
            + Statement = [
                + {
                    + Action   = [
                        + "ec2:Describe*",
                        + "s3:ListAllMyBuckets",
                      ]
                    + Effect   = "Allow"
                    + Resource = "*"
                  },
              ]
            + Version   = "2012-10-17"
          }
        )
      + policy_id           = (known after apply)
      + tags                = {
          + "Name" = "iampolicy_ammar"
        }
      + tags_all            = {
          + "Name" = "iampolicy_ammar"
        }
      + update_date         = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
````

---

## 🔍 Code Analysis

### Complete Configuration Structure

#### variables.tf
```hcl
# variables.tf - Configuration storage
variable "KKE_iampolicy" {
  description = "The name of the IAM policy to create"
  type        = string
  default     = "iampolicy_ammar"
}
```

#### main.tf
```hcl
# main.tf - Resource definition
resource "aws_iam_policy" "this" {
  name        = var.KKE_iampolicy      # References variable for naming
  description = "Custom IAM policy created for the Nautilus DevOps team"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:Describe*",             # EC2 describe permissions (wildcard)
          "s3:ListAllMyBuckets"        # S3 list all buckets permission
        ]
        Effect   = "Allow"             # Allow these actions
        Resource = "*"                 # Apply to all resources
      }
    ]
  })
  
  tags = {
    Name = var.KKE_iampolicy           # References variable for tagging
  }
}
```

### Variable Reference Pattern
````
variables.tf (definition)
    ↓
    var.KKE_iampolicy (reference in code)
    ↓
main.tf (usage in resources)
````

### IAM Policy Attributes
| Attribute | Value | Description |
|-----------|-------|-------------|
| **name** | iampolicy_ammar | The name of the IAM policy |
| **description** | Custom IAM policy... | Purpose of the policy |
| **policy** | JSON document | Policy permissions and statements |
| **path** | / | The path for the policy (default root path) |
| **tags** | dynamic | Uses variable for consistent identification |

### Policy Document Structure
````
Version: "2012-10-17"                      # IAM policy language version
Statement:                                 # Array of policy statements
  Action:                                  # AWS API actions
    - "ec2:Describe*"                     # All EC2 Describe actions
    - "s3:ListAllMyBuckets"               # S3 list all buckets action
  Effect: "Allow"                         # Action is permitted
  Resource: "*"                           # Applies to all resources
````

---

## ✅ Verification Steps

### Step 1: Verify Resources Created
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
````
aws_iam_policy.this
````

### Step 2: Check IAM Policy Details
```bash
# Show detailed policy configuration from Terraform state
terraform state show aws_iam_policy.this
```

### Step 3: Verify Variable Usage
```bash
# Display Terraform variables
terraform console
var.KKE_iampolicy
```

**Expected Output:**
````
"iampolicy_ammar"
````

---

## 🧪 Testing

### Verify IAM Policy Creation
```bash
# Show complete IAM policy configuration
terraform state show aws_iam_policy.this
```

### AWS CLI Verification
```bash
# Get specific policy details
aws iam get-policy --policy-arn arn:aws:iam::123456789012:policy/iampolicy_ammar
```

**Expected JSON Output:**
```json
{
    "Policy": {
        "PolicyName": "iampolicy_ammar",
        "PolicyId": "ANPAI23HZ27SIEXAMPLE",
        "Arn": "arn:aws:iam::123456789012:policy/iampolicy_ammar",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "Description": "Custom IAM policy created for the Nautilus DevOps team",
        "CreateDate": "2024-XX-XXTXX:XX:XX+00:00",
        "UpdateDate": "2024-XX-XXTXX:XX:XX+00:00",
        "Tags": [
            {
                "Key": "Name",
                "Value": "iampolicy_ammar"
            }
        ]
    }
}
```

### Verify Policy Document
```bash
# Get the policy version and document
aws iam get-policy-version --policy-arn arn:aws:iam::123456789012:policy/iampolicy_ammar --version-id v1
```

### Test Variable Override
```bash
# Apply with variable override
terraform apply -var="KKE_iampolicy=custom_iampolicy_ammar" -auto-approve
```

---

## 📚 Quick Reference

### File Structure
```bash
/home/bob/terraform/
├── main.tf          # IAM Policy resource definition
├── variables.tf     # Variable definitions
└── terraform.tfstate # State file (auto-generated)
```

### Complete Solution

#### variables.tf
```hcl
variable "KKE_iampolicy" {
  description = "The name of the IAM policy to create"
  type        = string
  default     = "iampolicy_ammar"
}
```

#### main.tf
```hcl
resource "aws_iam_policy" "this" {
  name        = var.KKE_iampolicy
  description = "Custom IAM policy created for the Nautilus DevOps team"
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:Describe*",
          "s3:ListAllMyBuckets"
        ]
        Effect   = "Allow"
        Resource = "*"
      }
    ]
  })
  
  tags = {
    Name = var.KKE_iampolicy
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
terraform state list
```

### Optional: Add Multiple Variables for Enhanced Configuration
```hcl
# Enhanced variables.tf with additional parameters
variable "KKE_iampolicy" {
  description = "The name of the IAM policy to create"
  type        = string
  default     = "iampolicy_ammar"
}

variable "policy_description" {
  description = "Description of the IAM policy"
  type        = string
  default     = "Custom IAM policy created for the Nautilus DevOps team"
}

variable "allowed_actions" {
  description = "List of allowed actions"
  type        = list(string)
  default = [
    "ec2:Describe*",
    "s3:ListAllMyBuckets"
  ]
}

variable "environment" {
  description = "Environment name"
  type        = string
  default     = "production"
}

variable "common_tags" {
  description = "Common tags for all resources"
  type        = map(string)
  default = {
    Project = "Nautilus"
    Team    = "DevOps"
  }
}
```

### Optional: Enhanced main.tf with Multiple Variables
```hcl
# Enhanced main.tf utilizing multiple variables
resource "aws_iam_policy" "this" {
  name        = var.KKE_iampolicy
  description = var.policy_description
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action   = var.allowed_actions
        Effect   = "Allow"
        Resource = "*"
      }
    ]
  })
  
  tags = merge(
    var.common_tags,
    {
      Name        = var.KKE_iampolicy
      Environment = var.environment
    }
  )
}
```

### Optional: Add Output Values
```hcl
# outputs.tf - Define output values
output "policy_name" {
  description = "Name of the created IAM policy"
  value       = aws_iam_policy.this.name
}

output "policy_arn" {
  description = "ARN of the created IAM policy"
  value       = aws_iam_policy.this.arn
}

output "policy_id" {
  description = "ID of the created IAM policy"
  value       = aws_iam_policy.this.policy_id
}

output "variable_value" {
  description = "Value of KKE_iampolicy variable"
  value       = var.KKE_iampolicy
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Variable not recognized**
- **Symptoms:** Error: "Unsupported attribute"
- **Solution:** Ensure variable is defined in variables.tf before use
```bash
# Check if variables.tf exists in correct location
ls -la /home/bob/terraform/variables.tf
```

**Issue 2: Invalid JSON in policy document**
- **Symptoms:** Error: "Invalid policy JSON"
- **Solution:** Verify jsonencode() syntax and nested structure
```hcl
# Test JSON encoding in terraform console
terraform console
jsonencode({ "key" = "value" })
```

**Issue 3: Invalid action syntax**
- **Symptoms:** Error about invalid actions
- **Solution:** Verify AWS action format (service:Action)
```hcl
# Valid action examples:
# "ec2:Describe*"           # Wildcard for all describe actions
# "s3:ListAllMyBuckets"     # Specific action
# "s3:*"                    # All S3 actions
```

**Issue 4: Policy already exists**
- **Symptoms:** Error: "EntityAlreadyExists"
- **Solution:** Import existing policy or use different variable value
```bash
# Import existing policy into Terraform state
terraform import aws_iam_policy.this arn:aws:iam::123456789012:policy/iampolicy_ammar
```

---

## 💡 Best Practices Applied

- **Configuration Separation:** Variables stored separately from resources
- **Reusability:** Parameterized configuration for multiple deployments
- **Naming Conventions:** Clear variable names matching requirements
- **Policy Documents:** JSON-encoded permission statements
- **Variable Management:** Centralized configuration in variables.tf
- **Infrastructure as Code:** Industry-standard IaC patterns

---

## 🚀 Production Considerations

### IAM Policy Security Framework
- **Least Privilege:** Grant only necessary permissions
- **Specific Actions:** Avoid using wildcards where possible
- **Resource Scope:** Limit policy to specific resources
- **Condition Blocks:** Add conditions for additional security
- **Regular Review:** Audit policy permissions periodically

### Production Policy Example
```hcl
# Production policy with more specific permissions
resource "aws_iam_policy" "this" {
  name        = var.KKE_iampolicy
  description = var.policy_description
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid    = "EC2ReadOnly"
        Action = [
          "ec2:Describe*",
          "ec2:Get*"
        ]
        Effect   = "Allow"
        Resource = "*"
      },
      {
        Sid    = "S3BucketList"
        Action = "s3:ListAllMyBuckets"
        Effect = "Allow"
        Resource = "arn:aws:s3:::*"
      }
    ]
  })
  
  tags = merge(
    var.common_tags,
    {
      Name        = var.KKE_iampolicy
      Environment = var.environment
    }
  )
}
```

### Variable Management Strategy
- **Environment-Specific Files:** Create terraform.tfvars for different environments
- **Sensitive Values:** Use sensitive flag for credentials and secrets
- **Validation:** Implement validation rules for variable values
- **Documentation:** Add descriptions to all variables

### Environment Configuration
```bash
# Use different .tfvars files for environments
terraform apply -var-file="dev.tfvars" -auto-approve
terraform apply -var-file="prod.tfvars" -auto-approve
```

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Terraform Variables:** Defining and using variables in configurations
- **Variable Types:** Understanding string type and type safety
- **Configuration Separation:** Organizing infrastructure code for maintainability
- **Variable References:** Proper syntax for referencing variables in resources
- **IAM Policy Management:** Creating parameterized IAM policy resources
- **Policy Documents:** Understanding and implementing JSON-encoded policies
- **JSON Encoding:** Using jsonencode() for policy documents

**Terraform Features Used:**
- `variable` blocks - Variable definition syntax
- `var.` prefix - Variable reference in code
- `default` values - Variable defaults
- `type` specification - Type safety
- `description` - Variable documentation
- `jsonencode()` - JSON policy encoding
- `aws_iam_policy` - IAM policy resource creation

---

## 🎯 Task Completion Summary

Successfully Completed:
- **Variable Definition** - Created `KKE_iampolicy` variable with default "iampolicy_ammar"
- **File Structure** - Organized code into `main.tf` and `variables.tf`
- **IAM Policy** - Created with variable-based naming
- **Policy Document** - Configured with EC2 and S3 permissions
- **Parameterization** - Configuration values stored as variables
- **Tagging** - Consistent identification using variable

**Final Status:** Task 40 completed successfully with all requirements met

**Policy Automation:** The parameterized IAM policy configuration is now ready for reuse across multiple deployments and teams, providing a foundation for the Nautilus DevOps team's automated access management strategy.

### 🎉 Congratulations - All 40 Tasks Completed!

You have successfully completed all 40 KodeKloud Terraform Level 1 tasks. This comprehensive journey has covered:

**Foundation (Tasks 1-5):** Key Pairs, Security Groups, VPC configurations
**Networking (Tasks 6-10):** Elastic IPs, EC2 instances, AMIs, EBS volumes, Snapshots
**Monitoring (Tasks 11-13):** CloudWatch alarms, Public/Private S3 buckets
**Identity (Tasks 14-15):** IAM Users, IAM Groups
**Advanced Identity (Tasks 16-40):** IAM Policies, DynamoDB, SNS, CloudFormation, Variables

You now possess production-ready Terraform knowledge for AWS infrastructure automation!
````