# 🌟 Task 35 - Create AWS VPC Using Terraform with Variables

## 📌 Task Description

The **Nautilus DevOps team** is automating the creation of AWS Virtual Private Clouds (VPCs) using Terraform to streamline networking management. They require a VPC to be created with specific configurations, utilizing variables for flexibility.

**Requirements:**
- Create an AWS VPC named **`datacenter-vpc`** using Terraform.
- Store the VPC name in a variable named `KKE_vpc`.
- Configure the VPC with a CIDR block of `10.0.0.0/16`.
- Store configuration values in a `variables.tf` file.
- Structure the Terraform script with a `main.tf` file referencing `variables.tf`.
- The Terraform working directory is **`/home/bob/terraform`**.

💡 **Note:** The VPC must be created with the specified name and CIDR block, using a variable for the name defined in `variables.tf`. The configuration must be verifiable via Terraform and AWS CLI. The current date and time is October 18, 2025, 06:09 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (VPC, us-east-1 region by default)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- VPC named `datacenter-vpc` with CIDR block `10.0.0.0/16`
**Working Directory:** `/home/bob/terraform`
**Configuration Files:**
- `main.tf`: Defines the VPC resource and references variables.
- `variables.tf`: Stores the `KKE_vpc` variable for the VPC name.

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **VPC Resource:** Creates the `datacenter-vpc` with the specified CIDR block.
- **Variable Management:** Uses `KKE_vpc` variable to define the VPC name, stored in `variables.tf`.
- **Terraform Configuration:** Separates resource definitions (`main.tf`) and variable definitions (`variables.tf`) for modularity.

### 🎯 Implementation Strategy
1. Verify the absence of the `datacenter-vpc` in the `us-east-1` region using AWS CLI.
2. Create or update `main.tf` to define the VPC using the `KKE_vpc` variable.
3. Create `variables.tf` to define the `KKE_vpc` variable with the default value `datacenter-vpc`.
4. Deploy the configuration to create the VPC.
5. Verify the VPC creation using Terraform state and AWS CLI.

---

## 🚀 Implementation Steps

### Step 1: Verify VPC Absence

Confirm that no VPC named `datacenter-vpc` exists in the `us-east-1` region using AWS CLI.

```bash
# Check for existing VPC
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc" --query "Vpcs[].{VpcId:VpcId,CidrBlock:CidrBlock}"
```

**Expected Output (if no VPC exists):**
```json
[]
```

**Note:** Ensure AWS CLI is configured with credentials that have VPC permissions.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Create/Update Terraform Configuration Files

#### Create `variables.tf`

Create or update the `variables.tf` file to define the `KKE_vpc` variable:

```hcl
# variables.tf

variable "KKE_vpc" {
  description = "The name of the VPC"
  type        = string
  default     = "datacenter-vpc"
}
```

**Variable Breakdown:**
- `KKE_vpc`: Stores the VPC name with a default value of `datacenter-vpc`.
- `description`: Provides context for the variable's purpose.
- `type`: Ensures the variable is a string.
- `default`: Sets the default name as required.

#### Update `main.tf`

Update the `main.tf` file to define the VPC using the `KKE_vpc` variable:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Create VPC
resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = var.KKE_vpc
  }
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider for the `us-east-1` region.
- `aws_vpc.this`: Creates a VPC with the CIDR block `10.0.0.0/16`.
- `tags`: Uses the `KKE_vpc` variable to set the VPC name to `datacenter-vpc`.

---

### Step 4: Initialize Terraform

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
# Format the configuration files
terraform fmt

# Validate the configuration
terraform validate
```

**Purpose:** Ensure code formatting consistency and validate syntax correctness for both `main.tf` and `variables.tf`.

**Expected Output:**
```
Success! The configuration is valid.
```

---

### Step 6: Plan and Apply Configuration

```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply -auto-approve
```

**Expected Output:**
```
Terraform will perform the following actions:

  # aws_vpc.this will be created
  + resource "aws_vpc" "this" {
      + cidr_block           = "10.0.0.0/16"
      + id                   = (known after apply)
      + tags                 = {
          + "Name" = "datacenter-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

### Step 7: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_vpc.this: Refreshing state... [id=vpc-0123456789abcdef0]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

### Step 8: Verify VPC Creation

Confirm the VPC `datacenter-vpc` exists using AWS CLI.

```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc" --query "Vpcs[].{VpcId:VpcId,CidrBlock:CidrBlock}"
```

**Expected Output:**
```json
[
    {
        "VpcId": "vpc-0123456789abcdef0",
        "CidrBlock": "10.0.0.0/16"
    }
]
```

---

## 🔍 Code Analysis

### Complete Configuration

#### `main.tf`
```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Create VPC
resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = var.KKE_vpc
  }
}
```

#### `variables.tf`
```hcl
variable "KKE_vpc" {
  description = "The name of the VPC"
  type        = string
  default     = "datacenter-vpc"
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_vpc.this.cidr_block** | 10.0.0.0/16 | CIDR block for the VPC |
| **aws_vpc.this.tags.Name** | var.KKE_vpc | References the `KKE_vpc` variable (`datacenter-vpc`) |
| **variable.KKE_vpc.default** | datacenter-vpc | Default name for the VPC |

### Resource Properties
- **Region:** `us-east-1` as specified for the AWS provider.
- **VPC Configuration:** Creates a VPC with CIDR block `10.0.0.0/16` and name `datacenter-vpc`.
- **Variable Usage:** Uses `KKE_vpc` for modularity and reusability.

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_vpc.this
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show details of the VPC
terraform state show aws_vpc.this
```

**Expected Output:**
```
# aws_vpc.this:
resource "aws_vpc" "this" {
    cidr_block           = "10.0.0.0/16"
    id                   = "vpc-0123456789abcdef0"
    tags                 = {
        "Name" = "datacenter-vpc"
    }
}
```

### Step 3: Verify in AWS Console (Optional)

```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

**Expected Output:**
```json
{
    "Vpcs": [
        {
            "CidrBlock": "10.0.0.0/16",
            "VpcId": "vpc-0123456789abcdef0",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "datacenter-vpc"
                }
            ]
        }
    ]
}
```

---

## 🧪 Testing

### Verify VPC Creation

1. Check the VPC details:
```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

**Expected Output:** Confirms the VPC exists with the correct CIDR block and name.

2. Test variable override (optional, for validation):
```bash
terraform apply -var "KKE_vpc=test-vpc" -auto-approve
```

**Note:** This would update the VPC name to `test-vpc`, verifying the variable's functionality. Revert to `datacenter-vpc` afterward if needed.

---

## 📚 Quick Reference

### Complete Solution

#### `main.tf`
```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Create VPC
resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = var.KKE_vpc
  }
}
```

#### `variables.tf`
```hcl
variable "KKE_vpc" {
  description = "The name of the VPC"
  type        = string
  default     = "datacenter-vpc"
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
terraform state show aws_vpc.this

# Verify via AWS CLI
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: VPC Already Exists**

- **Symptoms:** Error: "VpcLimitExceeded: The maximum number of VPCs has been reached" or "CIDR block already in use"
- **Solution:** Check for existing VPCs and import or delete if necessary
```bash
aws ec2 describe-vpcs --region us-east-1 --filters "Name=tag:Name,Values=datacenter-vpc"
terraform import aws_vpc.this vpc-0123456789abcdef0
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: ec2:CreateVpc"
- **Solution:** Ensure AWS credentials have VPC permissions
```bash
# Required permissions:
# - ec2:CreateVpc
# - ec2:DescribeVpcs
# - ec2:CreateTags
```

**Issue 3: Invalid CIDR Block**

- **Symptoms:** Error: "Invalid CIDR block"
- **Solution:** Verify the CIDR block is valid and not overlapping
```bash
# Ensure CIDR is 10.0.0.0/16 as specified
cat main.tf
```

**Issue 4: Variable Not Found**

- **Symptoms:** Error: "Reference to undeclared variable var.KKE_vpc"
- **Solution:** Ensure `variables.tf` exists and is correctly formatted
```bash
cat variables.tf
terraform validate
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Ensured credentials have least-privilege access for VPC operations.
- **📊 Resource Naming:** Used the `KKE_vpc` variable for flexible naming.
- **🏷️ Modularity:** Separated configuration (`main.tf`) and variables (`variables.tf`) for maintainability.
- **📍 Verifiability:** Ensured VPC creation is verifiable via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### VPC Creation Framework
- **Scalability:** Use variables for CIDR blocks and other parameters to support multiple VPCs.
- **Monitoring:** Enable AWS CloudTrail to log VPC actions for auditing.
- **Resource Tagging:** Add tags for cost allocation and tracking.
- **Networking:** Plan for subnets, route tables, and gateways for a complete VPC setup.

### Enhanced Configuration

```hcl
# main.tf
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "this" {
  cidr_block           = var.vpc_cidr_block
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name        = var.KKE_vpc
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "data-center"
  }
}

output "vpc_id" {
  description = "ID of the VPC"
  value       = aws_vpc.this.id
}
```

```hcl
# variables.tf
variable "KKE_vpc" {
  description = "The name of the VPC"
  type        = string
  default     = "datacenter-vpc"
}

variable "vpc_cidr_block" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}
```

### Security Strategy
- **Access Control:** Use IAM roles with specific VPC permissions.
- **Monitoring:** Log VPC actions with CloudTrail for audit trails.
- **Network Security:** Enable DNS support and hostnames for production readiness.
- **Validation:** Verify VPC configuration before deploying dependent resources.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **VPC Management:** Creating VPCs with Terraform.
- **Variable Usage:** Implementing variables for flexible configurations.
- **Modular Configuration:** Structuring Terraform code with separate `main.tf` and `variables.tf`.
- **State Verification:** Confirming VPC creation using Terraform and AWS CLI.

**Terraform Features Used:**
- `aws_vpc`: Creating and managing the VPC.
- `variable`: Defining reusable configuration parameters.
- AWS provider integration for VPC services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **VPC Created**: Created `datacenter-vpc` with CIDR block `10.0.0.0/16`.
- **Variable Used**: Stored VPC name in `KKE_vpc` variable in `variables.tf`.
- **File Structure**: Used `main.tf` and `variables.tf` as specified.
- **Verification**: Confirmed VPC creation via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Networking Foundation:** The `datacenter-vpc` has been created with the specified configuration, supporting the Nautilus DevOps team’s infrastructure automation efforts.

### 🔮 Resources Ready for:
- **Networking Expansion**: Add subnets, route tables, and gateways.
- **Monitoring**: Audit VPC actions with CloudTrail.
- **Scalability**: Use variables for additional VPC configurations.
- **Security**: Enhance with network ACLs and security groups.