````markdown
# 🌟 Task 37 - Elastic IP Variable Setup Using Terraforms

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the AWS cloud. As part of this phased migration approach, they need to allocate an Elastic IP address to support external access for specific workloads.

This task focuses on creating a parameterized Elastic IP configuration using Terraform variables, enabling reusable infrastructure code across multiple deployments and environments.

**Requirements:**
- Create an AWS Elastic IP using Terraform with variable-based configuration
- The Elastic IP name **`datacenter-eip`** should be stored in a variable named **`KKE_eip`**
- Configuration values should be stored in a **`variables.tf`** file
- The Terraform script should be structured with **`main.tf`** referencing **`variables.tf`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create separate files: `main.tf` and `variables.tf`

👉 **Your task:** Implement parameterized Elastic IP configuration using Terraform variables for enhanced infrastructure automation.

💡 **Note:** Using variables enables elastic IP configurations to be reused across different regions and environments.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (default region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- Elastic IP with variable-based configuration
- VPC-associated static public IP address
- Reusable infrastructure automation template

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Variables File:** Centralized configuration storage for EIP naming
- **Elastic IP Resource:** References variable for dynamic naming and management
- **VPC Association:** Configured for modern AWS VPC infrastructure
- **Infrastructure as Code:** Parameterized, reusable configuration pattern
- **Tagging Strategy:** Uses variable for consistent resource identification

### 🎯 Implementation Strategy
1. Define variable `KKE_eip` in `variables.tf` with default value
2. Create Elastic IP in `main.tf` referencing the variable
3. Configure VPC association for modern AWS deployments
4. Implement proper tagging using the variable

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

# Define variable for Elastic IP name
variable "KKE_eip" {
  description = "The name of the Elastic IP"
  type        = string
  default     = "datacenter-eip"
}
```

**Configuration Breakdown:**
- `variable "KKE_eip"` - Defines variable with name matching requirements
- `description` - Explains the variable's purpose for documentation
- `type = string` - Specifies variable data type
- `default = "datacenter-eip"` - Provides default value as specified in requirements

---

### Step 3: Create Main Configuration File

Create the `/home/bob/terraform/main.tf` file:
```hcl
# main.tf

# Create AWS Elastic IP with variable reference
resource "aws_eip" "this" {
  vpc = true
  
  tags = {
    Name = var.KKE_eip
  }
}
```

**Configuration Breakdown:**
- `aws_eip` - Creates Elastic IP resource
- `vpc = true` - Allocates EIP for VPC (modern AWS standard)
- `tags` - Assigns tags with variable reference for EIP identification
- `Name = var.KKE_eip` - References variable for dynamic naming

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

  # aws_eip.this will be created
  + resource "aws_eip" "this" {
      + allocation_id        = (known after apply)
      + association_id       = (known after apply)
      + carrier_ip           = (known after apply)
      + customer_owned_ip    = (known after apply)
      + domain               = (known after apply)
      + id                   = (known after apply)
      + instance             = (known after apply)
      + network_border_group = (known after apply)
      + network_interface    = (known after apply)
      + private_dns          = (known after apply)
      + private_ip           = (known after apply)
      + public_dns           = (known after apply)
      + public_ip            = (known after apply)
      + public_ipv4_pool     = "amazon"
      + tags                 = {
          + "Name" = "datacenter-eip"
        }
      + tags_all             = {
          + "Name" = "datacenter-eip"
        }
      + vpc                  = true
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
variable "KKE_eip" {
  description = "The name of the Elastic IP"
  type        = string
  default     = "datacenter-eip"
}
```

#### main.tf
```hcl
# main.tf - Resource definition
resource "aws_eip" "this" {
  vpc = true                           # VPC association flag
  
  tags = {
    Name = var.KKE_eip                # References variable for naming
  }
}
```

### Variable Reference Pattern
````
variables.tf (definition)
    ↓
    var.KKE_eip (reference in code)
    ↓
main.tf (usage in resources)
````

### Elastic IP Attributes
| Attribute | Value | Description |
|-----------|-------|-------------|
| **vpc** | true | Allocates EIP for use with instances in VPC |
| **domain** | vpc (default) | EIP domain (vpc or standard) |
| **tags** | dynamic | Uses variable for consistent naming |

---

## ✅ Verification Steps

### Step 1: Verify Resources Created
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
````
aws_eip.this
````

### Step 2: Check Elastic IP Details
```bash
# Show detailed configuration from Terraform state
terraform state show aws_eip.this
```

### Step 3: Verify Variable Usage
```bash
# Display Terraform variables
terraform console
var.KKE_eip
```

**Expected Output:**
````
"datacenter-eip"
````

---

## 🧪 Testing

### Verify Elastic IP Creation
```bash
# Show complete EIP configuration
terraform state show aws_eip.this
```

### AWS CLI Verification
```bash
# List Elastic IPs with matching tag
aws ec2 describe-addresses --filters "Name=tag:Name,Values=datacenter-eip"
```

**Expected JSON Output:**
```json
{
    "Addresses": [
        {
            "InstanceId": "",
            "PublicIp": "X.X.X.X",
            "AllocationId": "eipalloc-xxxxxxxxxxxxxxxxx",
            "AssociationId": "",
            "Domain": "vpc",
            "NetworkInterfaceId": "",
            "NetworkInterfaceOwnerId": "",
            "PrivateIpAddress": "",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "datacenter-eip"
                }
            ],
            "PublicIpv4Pool": "amazon",
            "NetworkBorderGroup": "us-east-1",
            "CustomerOwnedIp": "",
            "CustomerOwnedIpv4Pool": "",
            "CarrierIp": ""
        }
    ]
}
```

### Test Variable Override
```bash
# Apply with variable override
terraform apply -var="KKE_eip=custom-datacenter-eip" -auto-approve
```

---

## 📚 Quick Reference

### File Structure
```bash
/home/bob/terraform/
├── main.tf          # Elastic IP resource definition
├── variables.tf     # Variable definitions
└── terraform.tfstate # State file (auto-generated)
```

### Complete Solution

#### variables.tf
```hcl
variable "KKE_eip" {
  description = "The name of the Elastic IP"
  type        = string
  default     = "datacenter-eip"
}
```

#### main.tf
```hcl
resource "aws_eip" "this" {
  vpc = true
  
  tags = {
    Name = var.KKE_eip
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
variable "KKE_eip" {
  description = "The name of the Elastic IP"
  type        = string
  default     = "datacenter-eip"
}

variable "eip_domain" {
  description = "Domain for Elastic IP (vpc or standard)"
  type        = string
  default     = "vpc"
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
resource "aws_eip" "this" {
  vpc = var.eip_domain == "vpc" ? true : false
  
  tags = merge(
    var.common_tags,
    {
      Name        = var.KKE_eip
      Environment = var.environment
    }
  )
}
```

### Optional: Add Output Values
```hcl
# outputs.tf - Define output values
output "eip_id" {
  description = "ID (allocation ID) of the Elastic IP"
  value       = aws_eip.this.id
}

output "eip_public_ip" {
  description = "Public IP address of the Elastic IP"
  value       = aws_eip.this.public_ip
}

output "eip_domain" {
  description = "Domain of the Elastic IP"
  value       = aws_eip.this.domain
}

output "eip_name" {
  description = "Name of the Elastic IP"
  value       = var.KKE_eip
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
variable "KKE_eip" {
  type = string  # Must be string
}
```

**Issue 3: Variable not applied correctly**
- **Symptoms:** EIP tag doesn't match variable value
- **Solution:** Ensure var. prefix is used when referencing variable
```hcl
# Correct usage
Name = var.KKE_eip

# Incorrect usage (treats as literal string)
Name = KKE_eip
```

**Issue 4: EIP limit exceeded**
- **Symptoms:** Error: "AddressLimitExceeded"
- **Solution:** AWS limits EIPs per region (default 5)
```bash
# Check existing EIPs
aws ec2 describe-addresses --query 'Addresses[*].[PublicIp,AllocationId,Tags]' --output table
```

---

## 💡 Best Practices Applied

- **Configuration Separation:** Variables stored separately from resources
- **Reusability:** Parameterized configuration for multiple deployments
- **Naming Conventions:** Clear variable names matching requirements
- **Variable Management:** Centralized configuration in variables.tf
- **Infrastructure as Code:** Industry-standard IaC patterns

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
variable "KKE_eip" {
  description = "The name of the Elastic IP"
  type        = string
  default     = "datacenter-eip"
  
  validation {
    condition     = can(regex("^[a-zA-Z0-9_-]+$", var.KKE_eip))
    error_message = "EIP name must contain only alphanumeric characters, underscores, and hyphens."
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
- **Elastic IP Management:** Creating parameterized EIP resources

**Terraform Features Used:**
- `variable` blocks - Variable definition syntax
- `var.` prefix - Variable reference in code
- `default` values - Variable defaults
- `type` specification - Type safety
- `description` - Variable documentation

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Variable Definition** - Created `KKE_eip` variable with default "datacenter-eip"
- **File Structure** - Organized code into `main.tf` and `variables.tf`
- **Elastic IP** - Created with variable-based naming
- **Parameterization** - Configuration values stored as variables
- **VPC Association** - Configured for modern AWS infrastructure

**Final Status:** Task completed successfully with all requirements met

**Infrastructure Automation:** The parameterized Elastic IP configuration is now ready for reuse across multiple deployments and environments, providing a foundation for the Nautilus DevOps team's phased cloud migration strategy.

### 🔮 Configuration Ready for:
- **Multiple Deployments:** Reuse configuration across environments
- **Dynamic Naming:** Change EIP name via variable override
- **Environment Scaling:** Extend with additional environment-specific variables
- **Team Collaboration:** Share standardized infrastructure templates
- **Migration Planning:** Foundation for migrating workloads requiring static IPs
````