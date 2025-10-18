````markdown
# 🌟 Task 36 - Security Group Variable Setup Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is enhancing infrastructure automation and needs to provision a Security Group using Terraform with specific configurations. This task focuses on implementing infrastructure as code best practices by utilizing variables to make configurations reusable, maintainable, and dynamic.

Using variables in Terraform allows teams to separate configuration from code, making it easier to manage different environments and reducing code duplication across projects.

**Requirements:**
- Create an AWS Security Group using Terraform with variable-based configuration
- The Security Group name **`devops-sg`** should be stored in a variable named **`KKE_sg`**
- Configuration values should be stored in a **`variables.tf`** file
- The Terraform script should be structured with **`main.tf`** referencing **`variables.tf`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create separate files: `main.tf` and `variables.tf`

👉 **Your task:** Implement parameterized Security Group configuration using Terraform variables for enhanced infrastructure automation.

💡 **Note:** Using variables enables infrastructure code reusability across different environments and configurations.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (default region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- Security Group with variable-based configuration
- Parameterized security rules (SSH, egress)
- Reusable infrastructure automation template

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Variables File:** Centralized configuration storage for reusable values
- **Security Group Resource:** References variable for dynamic naming
- **Ingress Rules:** SSH access configuration
- **Egress Rules:** Outbound traffic allowance
- **Infrastructure as Code:** Parameterized, reusable configuration pattern

### 🎯 Implementation Strategy
1. Define variable `KKE_sg` in `variables.tf` with default value
2. Create Security Group in `main.tf` referencing the variable
3. Configure ingress and egress rules with practical defaults
4. Structure files for maximum reusability and maintainability

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

# Define variable for Security Group name
variable "KKE_sg" {
  description = "The name of the Security Group"
  type        = string
  default     = "devops-sg"
}
```

**Configuration Breakdown:**
- `variable "KKE_sg"` - Defines variable with name matching requirements
- `description` - Explains the variable's purpose
- `type = string` - Specifies variable data type
- `default = "devops-sg"` - Provides default value as specified

---

### Step 3: Create Main Configuration File

Create the `/home/bob/terraform/main.tf` file:
```hcl
# main.tf

# Create AWS Security Group with variable reference
resource "aws_security_group" "this" {
  name        = var.KKE_sg
  description = "Security group for DevOps environment"
  
  # Ingress rule: Allow SSH access
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  # Egress rule: Allow all outbound traffic
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = var.KKE_sg
  }
}
```

**Configuration Breakdown:**
- `aws_security_group` - Creates Security Group resource
- `name = var.KKE_sg` - References variable for dynamic naming
- `ingress` block - Defines inbound SSH (port 22) rule
- `egress` block - Defines outbound traffic allowance
- `tags` - Uses variable for resource tagging

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

  # aws_security_group.this will be created
  + resource "aws_security_group" "this" {
      + arn                    = (known after apply)
      + description            = "Security group for DevOps environment"
      + egress                 = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + from_port        = 0
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "-1"
              + security_groups  = []
              + to_port          = 0
              + user_id          = (known after apply)
            },
        ]
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = "Allow SSH"
              + from_port        = 22
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + to_port          = 22
              + user_id          = (known after apply)
            },
        ]
      + name                   = "devops-sg"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + tags                   = {
          + "Name" = "devops-sg"
        }
      + tags_all               = {
          + "Name" = "devops-sg"
        }
      + vpc_id                 = (known after apply)
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
variable "KKE_sg" {
  description = "The name of the Security Group"
  type        = string
  default     = "devops-sg"
}
```

#### main.tf
```hcl
# main.tf - Resource definition
resource "aws_security_group" "this" {
  name        = var.KKE_sg              # References variable
  description = "Security group for DevOps environment"
  
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = var.KKE_sg                   # References variable
  }
}
```

### Variable Reference Pattern
````
variables.tf (definition)
    ↓
    var.KKE_sg (reference in code)
    ↓
main.tf (usage in resources)
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
aws_security_group.this
````

### Step 2: Check Security Group Details
```bash
# Show detailed configuration from Terraform state
terraform state show aws_security_group.this
```

### Step 3: Verify Variable Usage
```bash
# Display Terraform variables
terraform console
var.KKE_sg
```

**Expected Output:**
````
"devops-sg"
````

---

## 🧪 Testing

### Verify Security Group Creation
```bash
# Show complete security group configuration
terraform state show aws_security_group.this
```

### AWS CLI Verification
```bash
# List security groups with matching name
aws ec2 describe-security-groups --filters "Name=group-name,Values=devops-sg"
```

**Expected JSON Output:**
```json
{
    "SecurityGroups": [
        {
            "GroupName": "devops-sg",
            "GroupId": "sg-xxxxxxxxx",
            "Description": "Security group for DevOps environment",
            "IpPermissions": [
                {
                    "FromPort": 22,
                    "ToPort": 22,
                    "IpProtocol": "tcp",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0", "Description": "Allow SSH"}]
                }
            ],
            "IpPermissionsEgress": [
                {
                    "IpProtocol": "-1",
                    "FromPort": 0,
                    "ToPort": 0,
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}]
                }
            ]
        }
    ]
}
```

### Test Variable Override
```bash
# Apply with variable override
terraform apply -var="KKE_sg=custom-devops-sg" -auto-approve
```

---

## 📚 Quick Reference

### File Structure
```bash
/home/bob/terraform/
├── main.tf          # Security Group resource definition
├── variables.tf     # Variable definitions
└── terraform.tfstate # State file (auto-generated)
```

### Complete Solution

#### variables.tf
```hcl
variable "KKE_sg" {
  description = "The name of the Security Group"
  type        = string
  default     = "devops-sg"
}
```

#### main.tf
```hcl
resource "aws_security_group" "this" {
  name        = var.KKE_sg
  description = "Security group for DevOps environment"
  
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = var.KKE_sg
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

### Optional: Add Multiple Variables
```hcl
# Enhanced variables.tf with additional parameters
variable "KKE_sg" {
  description = "The name of the Security Group"
  type        = string
  default     = "devops-sg"
}

variable "allowed_ssh_cidr" {
  description = "CIDR block allowed for SSH"
  type        = string
  default     = "0.0.0.0/0"
}

variable "environment" {
  description = "Environment name"
  type        = string
  default     = "production"
}

variable "tags" {
  description = "Additional tags for resources"
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
resource "aws_security_group" "this" {
  name        = var.KKE_sg
  description = "Security group for DevOps environment"
  
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [var.allowed_ssh_cidr]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags = merge(
    var.tags,
    {
      Name        = var.KKE_sg
      Environment = var.environment
    }
  )
}
```

### Optional: Add Output Values
```hcl
# outputs.tf - Define output values
output "security_group_id" {
  description = "ID of the created security group"
  value       = aws_security_group.this.id
}

output "security_group_name" {
  description = "Name of the created security group"
  value       = aws_security_group.this.name
}

output "security_group_arn" {
  description = "ARN of the created security group"
  value       = aws_security_group.this.arn
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

**Issue 2: Type mismatch for variable**
- **Symptoms:** Error: "Invalid type conversion"
- **Solution:** Ensure variable type matches usage
```hcl
# Verify variable type definition
variable "KKE_sg" {
  type = string  # Must be string, not list or number
}
```

**Issue 3: Variable not applied correctly**
- **Symptoms:** Security group name doesn't match variable value
- **Solution:** Ensure var. prefix is used when referencing variable
```hcl
# Correct usage
name = var.KKE_sg

# Incorrect usage (treats as literal string)
name = KKE_sg
```

**Issue 4: Default value not used**
- **Symptoms:** Variable requires input during apply
- **Solution:** Ensure default value is specified in variables.tf
```hcl
variable "KKE_sg" {
  default = "devops-sg"  # Must have default value
}
```

---

## 💡 Best Practices Applied

- **🔐 Configuration Separation:** Variables stored separately from resources
- **📊 Reusability:** Parameterized configuration for multiple deployments
- **🏷️ Naming Conventions:** Clear variable names matching requirements
- **🔄 Variable Management:** Centralized configuration in variables.tf
- **📍 Infrastructure as Code:** Industry-standard IaC patterns

---

## 🚀 Production Considerations

### Variable Management Strategy
- **Environment-Specific Files:** Create terraform.tfvars for different environments
- **Sensitive Values:** Use sensitive flag for credentials and secrets
- **Validation:** Implement validation rules for variable values
- **Documentation:** Add descriptions to all variables

### Production Variables Pattern
```hcl
# variables.tf - Production setup
variable "KKE_sg" {
  description = "The name of the Security Group"
  type        = string
  default     = "devops-sg"
  
  validation {
    condition     = can(regex("^[a-zA-Z0-9_-]+$", var.KKE_sg))
    error_message = "Security group name must contain only alphanumeric characters, underscores, and hyphens."
  }
}
```

### Environment Configuration
```bash
# Use different .tfvars files for environments
terraform apply -var-file="dev.tfvars"
terraform apply -var-file="prod.tfvars"
```

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Terraform Variables:** Defining and using variables in configurations
- **Variable Types:** Understanding string type and type safety
- **Configuration Separation:** Organizing infrastructure code for maintainability
- **Variable References:** Proper syntax for referencing variables in resources

**Terraform Features Used:**
- `variable` blocks - Variable definition syntax
- `var.` prefix - Variable reference in code
- `default` values - Variable defaults
- `type` specification - Type safety
- `description` - Variable documentation

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Variable Definition** - Created `KKE_sg` variable with default "devops-sg"
- **File Structure** - Organized code into `main.tf` and `variables.tf`
- **Security Group** - Created with variable-based naming
- **Parameterization** - Configuration values stored as variables
- **File Structure** - Used separate files as specified

**Final Status:** Task completed successfully with all requirements met

**Infrastructure Automation:** The parameterized Security Group configuration is now ready for reuse across multiple environments and deployments, providing a foundation for the Nautilus DevOps team's enhanced infrastructure automation strategy.

### Next Steps:
- Extend variables for HTTP/HTTPS ports
- Add environment-specific variable files
- Implement variable validation
- Create additional outputs for resource reference
````