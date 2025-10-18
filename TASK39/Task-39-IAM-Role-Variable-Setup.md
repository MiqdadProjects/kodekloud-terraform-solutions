````markdown
# 🌟 Task 39 - IAM Role Variable Setup Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is automating IAM role creation using Terraform to streamline permissions management. This task focuses on implementing parameterized IAM role configurations that enable consistent, reusable role provisioning across the organization.

Using Terraform variables for IAM role creation allows teams to maintain consistent naming conventions, simplify role provisioning workflows, and enable easy deployment across multiple environments with standardized trust relationships.

**Requirements:**
- Create an AWS IAM role using Terraform with variable-based configuration
- The IAM role name **`iamrole_ravi`** should be stored in a variable named **`KKE_iamrole`**
- Configuration values should be stored in a **`variables.tf`** file
- The Terraform script should be structured with **`main.tf`** referencing **`variables.tf`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create separate files: `main.tf` and `variables.tf`
- Include assume role policy for EC2 service principal

👉 **Your task:** Implement parameterized IAM role configuration using Terraform variables for enhanced role management automation.

💡 **Note:** Using variables enables IAM role configurations to be reused across different services and environments with consistent trust policies.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (global service)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- IAM Role with variable-based configuration
- Assume role policy for EC2 service integration
- Parameterized role provisioning template
- Reusable trust relationship configuration

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Variables File:** Centralized configuration storage for IAM role naming
- **IAM Role Resource:** References variable for dynamic role creation
- **Assume Role Policy:** Defines trust relationship with EC2 service
- **Service Principal:** EC2.amazonaws.com for EC2 instance attachment
- **Infrastructure as Code:** Parameterized, reusable configuration pattern
- **Tagging Strategy:** Uses variable for consistent resource identification

### 🎯 Implementation Strategy
1. Define variable `KKE_iamrole` in `variables.tf` with default value
2. Create IAM Role in `main.tf` with assume role policy
3. Configure trust relationship for EC2 service principal
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

# Define variable for IAM role name
variable "KKE_iamrole" {
  description = "The name of the IAM role to create"
  type        = string
  default     = "iamrole_ravi"
}
```

**Configuration Breakdown:**
- `variable "KKE_iamrole"` - Defines variable with name matching requirements
- `description` - Explains the variable's purpose for documentation
- `type = string` - Specifies variable data type
- `default = "iamrole_ravi"` - Provides default value as specified in requirements

---

### Step 3: Create Main Configuration File

Create the `/home/bob/terraform/main.tf` file:
```hcl
# main.tf

# Create AWS IAM Role with variable reference
resource "aws_iam_role" "this" {
  name = var.KKE_iamrole
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
  
  tags = {
    Name = var.KKE_iamrole
  }
}
```

**Configuration Breakdown:**
- `aws_iam_role` - Creates IAM Role resource
- `name = var.KKE_iamrole` - References variable for dynamic role naming
- `assume_role_policy` - Defines trust policy for the role
- `jsonencode()` - Encodes the policy document as JSON
- `Version = "2012-10-17"` - IAM policy language version
- `Statement` - Policy statement array
- `Effect = "Allow"` - Permits the specified action
- `Principal = { Service = "ec2.amazonaws.com" }` - EC2 service can assume this role
- `Action = "sts:AssumeRole"` - Allows assuming the role
- `tags` - Assigns tags with variable reference for role identification

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

  # aws_iam_role.this will be created
  + resource "aws_iam_role" "this" {
      + arn                   = (known after apply)
      + assume_role_policy    = jsonencode(
          {
            + Statement = [
                + {
                    + Action    = "sts:AssumeRole"
                    + Effect    = "Allow"
                    + Principal = {
                        + Service = "ec2.amazonaws.com"
                      }
                  },
              ]
            + Version   = "2012-10-17"
          }
        )
      + create_date           = (known after apply)
      + id                    = (known after apply)
      + inline_policy_names   = (known after apply)
      + managed_policy_arns   = (known after apply)
      + max_session_duration  = 3600
      + name                  = "iamrole_ravi"
      + path                  = "/"
      + role_last_updated     = (known after apply)
      + tags                  = {
          + "Name" = "iamrole_ravi"
        }
      + tags_all              = {
          + "Name" = "iamrole_ravi"
        }
      + unique_id             = (known after apply)
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
variable "KKE_iamrole" {
  description = "The name of the IAM role to create"
  type        = string
  default     = "iamrole_ravi"
}
```

#### main.tf
```hcl
# main.tf - Resource definition
resource "aws_iam_role" "this" {
  name = var.KKE_iamrole                    # References variable for naming
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"     # EC2 can assume this role
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
  
  tags = {
    Name = var.KKE_iamrole                  # References variable for tagging
  }
}
```

### Variable Reference Pattern
````
variables.tf (definition)
    ↓
    var.KKE_iamrole (reference in code)
    ↓
main.tf (usage in resources)
````

### IAM Role Attributes
| Attribute | Value | Description |
|-----------|-------|-------------|
| **name** | iamrole_ravi | The name of the IAM role |
| **assume_role_policy** | JSON policy | Trust policy defining who can assume the role |
| **path** | / | The path for the role (default root path) |
| **tags** | dynamic | Uses variable for consistent identification |
| **max_session_duration** | 3600 | Maximum session duration in seconds (default 1 hour) |

### Assume Role Policy Breakdown
````
Version: "2012-10-17"                       # IAM policy language version
Statement:                                  # Array of policy statements
  Effect: "Allow"                          # Action is permitted
  Principal: 
    Service: "ec2.amazonaws.com"           # EC2 service can perform action
  Action: "sts:AssumeRole"                 # Permission to assume this role
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
aws_iam_role.this
````

### Step 2: Check IAM Role Details
```bash
# Show detailed role configuration from Terraform state
terraform state show aws_iam_role.this
```

### Step 3: Verify Variable Usage
```bash
# Display Terraform variables
terraform console
var.KKE_iamrole
```

**Expected Output:**
````
"iamrole_ravi"
````

---

## 🧪 Testing

### Verify IAM Role Creation
```bash
# Show complete IAM role configuration
terraform state show aws_iam_role.this
```

### AWS CLI Verification
```bash
# Get specific role details
aws iam get-role --role-name iamrole_ravi
```

**Expected JSON Output:**
```json
{
    "Role": {
        "RoleName": "iamrole_ravi",
        "RoleId": "AIDAQ2BEXAMPLE",
        "Arn": "arn:aws:iam::123456789012:role/iamrole_ravi",
        "CreateDate": "2024-XX-XXTXX:XX:XX+00:00",
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
        },
        "Tags": [
            {
                "Key": "Name",
                "Value": "iamrole_ravi"
            }
        ]
    }
}
```

### Verify Assume Role Policy
```bash
# Get the role's assume role policy document
aws iam get-role-policy --role-name iamrole_ravi --policy-name trust-policy
```

### Test Variable Override
```bash
# Apply with variable override
terraform apply -var="KKE_iamrole=custom_iamrole_ravi" -auto-approve
```

---

## 📚 Quick Reference

### File Structure
```bash
/home/bob/terraform/
├── main.tf          # IAM Role resource definition
├── variables.tf     # Variable definitions
└── terraform.tfstate # State file (auto-generated)
```

### Complete Solution

#### variables.tf
```hcl
variable "KKE_iamrole" {
  description = "The name of the IAM role to create"
  type        = string
  default     = "iamrole_ravi"
}
```

#### main.tf
```hcl
resource "aws_iam_role" "this" {
  name = var.KKE_iamrole
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
  
  tags = {
    Name = var.KKE_iamrole
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
variable "KKE_iamrole" {
  description = "The name of the IAM role to create"
  type        = string
  default     = "iamrole_ravi"
}

variable "role_path" {
  description = "Path for the IAM role"
  type        = string
  default     = "/"
}

variable "max_session_duration" {
  description = "Maximum session duration in seconds"
  type        = number
  default     = 3600
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

### Optional: Enhanced main.tf with Multiple Variables and Services
```hcl
# Enhanced main.tf supporting multiple service principals
resource "aws_iam_role" "this" {
  name               = var.KKE_iamrole
  path               = var.role_path
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = [
            "ec2.amazonaws.com",
            "lambda.amazonaws.com"
          ]
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
  
  max_session_duration = var.max_session_duration
  
  tags = merge(
    var.common_tags,
    {
      Name        = var.KKE_iamrole
      Environment = var.environment
    }
  )
}
```

### Optional: Add Output Values
```hcl
# outputs.tf - Define output values
output "role_name" {
  description = "Name of the created IAM role"
  value       = aws_iam_role.this.name
}

output "role_arn" {
  description = "ARN of the created IAM role"
  value       = aws_iam_role.this.arn
}

output "role_id" {
  description = "ID of the created IAM role"
  value       = aws_iam_role.this.id
}

output "variable_value" {
  description = "Value of KKE_iamrole variable"
  value       = var.KKE_iamrole
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

**Issue 2: Invalid JSON in assume role policy**
- **Symptoms:** Error: "Invalid JSON"
- **Solution:** Verify jsonencode() syntax and nested structure
```hcl
# Test JSON encoding in terraform console
terraform console
jsonencode({ "key" = "value" })
```

**Issue 3: Service principal syntax error**
- **Symptoms:** Error about principal format
- **Solution:** Ensure service principal uses correct AWS service name
```hcl
# Valid service principals:
# "ec2.amazonaws.com"
# "lambda.amazonaws.com"
# "rds.amazonaws.com"
# "ecs-tasks.amazonaws.com"
```

**Issue 4: Role already exists**
- **Symptoms:** Error: "EntityAlreadyExists"
- **Solution:** Import existing role or use different variable value
```bash
# Import existing role into Terraform state
terraform import aws_iam_role.this iamrole_ravi
```

---

## 💡 Best Practices Applied

- **Configuration Separation:** Variables stored separately from resources
- **Reusability:** Parameterized configuration for multiple deployments
- **Naming Conventions:** Clear variable names matching requirements
- **Trust Policies:** Explicit service principal trust relationships
- **Variable Management:** Centralized configuration in variables.tf
- **Infrastructure as Code:** Industry-standard IaC patterns

---

## 🚀 Production Considerations

### IAM Role Security Framework
- **Trust Relationships:** Define explicit principals allowed to assume role
- **Session Duration:** Configure appropriate session timeouts
- **Policy Attachment:** Attach least-privilege policies to role
- **Monitoring:** Enable CloudTrail logging for role assumption
- **Regular Review:** Audit role permissions and trust relationships

### Variable Management Strategy
- **Environment-Specific Files:** Create terraform.tfvars for different environments
- **Sensitive Values:** Use sensitive flag for credentials and secrets
- **Validation:** Implement validation rules for variable values
- **Documentation:** Add descriptions to all variables

### Production Variables Pattern
```hcl
# variables.tf - Production setup
variable "KKE_iamrole" {
  description = "The name of the IAM role to create"
  type        = string
  default     = "iamrole_ravi"
  
  validation {
    condition     = can(regex("^[a-zA-Z0-9_+=,.@-]+$", var.KKE_iamrole))
    error_message = "Role name must contain only valid IAM role characters."
  }
}
```

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
- **IAM Role Management:** Creating parameterized IAM role resources
- **Trust Policies:** Understanding and implementing assume role policies
- **JSON Encoding:** Using jsonencode() for policy documents

**Terraform Features Used:**
- `variable` blocks - Variable definition syntax
- `var.` prefix - Variable reference in code
- `default` values - Variable defaults
- `type` specification - Type safety
- `description` - Variable documentation
- `jsonencode()` - JSON policy encoding
- `aws_iam_role` - IAM role resource creation

---

## 🎯 Task Completion Summary

Successfully Completed:
- **Variable Definition** - Created `KKE_iamrole` variable with default "iamrole_ravi"
- **File Structure** - Organized code into `main.tf` and `variables.tf`
- **IAM Role** - Created with variable-based naming
- **Trust Policy** - Configured with EC2 service principal
- **Parameterization** - Configuration values stored as variables
- **Tagging** - Consistent identification using variable

**Final Status:** Task completed successfully with all requirements met

**Role Automation:** The parameterized IAM role configuration is now ready for reuse across multiple deployments and services, providing a foundation for the Nautilus DevOps team's automated role management strategy.

### Next Steps:
- Attach policies to the created role
- Configure additional service principals
- Create instance profiles for EC2 attachment
- Implement role assumption auditing
````