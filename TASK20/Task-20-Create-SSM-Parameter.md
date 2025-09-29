# 🌟 Task 20 - Create AWS SSM Parameter Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** needs to create an AWS Systems Manager (SSM) parameter to securely store configuration data for their applications. The SSM parameter will be used to manage and retrieve configuration values in a centralized manner, ensuring secure and scalable access.

**Requirements:**
- Create an SSM parameter named **`datacenter-ssm-parameter`** using Terraform.
- Set the parameter type to **String**.
- Set the parameter value to **`datacenter-value`**.
- Create the parameter in the **us-east-1** region.
- Ensure the parameter is successfully created and can be retrieved.
- The Terraform working directory is **`/home/bob/terraform`**.
- Create the **`main.tf`** file (do not create a different `.tf` file).

👉 **Your task:** Create an SSM parameter using Terraform to store configuration data for the Nautilus DevOps team’s applications, ensuring it is accessible in the specified region.

💡 **Note:** AWS SSM Parameter Store is a fully managed service for storing and retrieving configuration data and secrets. The task is performed in the `/home/bob/terraform` directory, and the parameter must be verifiable via Terraform and AWS CLI. The current date and time is September 29, 2025, 11:14 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (SSM Parameter Store, us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- SSM parameter for storing configuration data
- String type parameter with a specific value
- Region-specific configuration (us-east-1)
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **SSM Parameter Resource:** Creates a parameter for storing configuration data.
- **Parameter Name:** Configures the parameter as `datacenter-ssm-parameter`.
- **Parameter Type:** Sets to `String` for simple text storage.
- **Parameter Value:** Assigns `datacenter-value` as the stored value.
- **Region:** Ensures creation in `us-east-1`.

### 🎯 Implementation Strategy
1. Create an SSM parameter with the specified name, type, and value.
2. Configure the Terraform provider to use the `us-east-1` region.
3. Deploy the parameter using Terraform in the specified directory.
4. Verify the parameter’s creation and retrieve its value using Terraform state and AWS CLI.

---

## 🚀 Implementation Steps

### Step 1: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 2: Create Main Terraform Configuration

Create the `main.tf` file with the complete solution:

```hcl
# main.tf

# Configure AWS provider for us-east-1 region
provider "aws" {
  region = "us-east-1"
}

# Create SSM parameter for storing configuration data
resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"
}
```

**Configuration Breakdown:**
- `provider "aws"`: Specifies the AWS provider with `us-east-1` region.
- `aws_ssm_parameter`: Defines an SSM parameter resource.
- `name = "datacenter-ssm-parameter"`: Sets the parameter name as required.
- `type = "String"`: Configures the parameter as a String type.
- `value = "datacenter-value"`: Assigns the specified value.

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

  # aws_ssm_parameter.datacenter_ssm_parameter will be created
  + resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + name             = "datacenter-ssm-parameter"
      + type             = "String"
      + value            = "datacenter-value"
      + version          = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

### Step 6: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_ssm_parameter.datacenter_ssm_parameter: Refreshing state... [id=datacenter-ssm-parameter]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure AWS provider for us-east-1 region
provider "aws" {
  region = "us-east-1"
}

# Create SSM parameter for storing configuration data
resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"
}
```

### SSM Parameter Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **name** | datacenter-ssm-parameter | The name of the SSM parameter |
| **type** | String | Type of the parameter (String, SecureString, or StringList) |
| **value** | datacenter-value | The value stored in the parameter |
| **tier** | Standard | Default tier for the parameter (Standard, Advanced, or Intelligent-Tiering) |
| **overwrite** | false | Default behavior for handling existing parameters |
| **tags** | {} | No tags defined (optional for future use) |

### SSM Parameter Default Properties
- **Tier:** `Standard` (suitable for most use cases, supports up to 4 KB of data).
- **Encryption:** Not enabled for `String` type (use `SecureString` for encryption).
- **Access Policy:** Controlled by IAM permissions for the AWS account.
- **Versioning:** Automatically increments with each update.

---

## ✅ Verification Steps

### Step 1: Verify Parameter Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_ssm_parameter.datacenter_ssm_parameter
```

### Step 2: Check Parameter Details in Terraform State

```bash
# Show detailed parameter information from Terraform state
terraform state show aws_ssm_parameter.datacenter_ssm_parameter
```

**Expected Output:**
```
# aws_ssm_parameter.datacenter_ssm_parameter:
resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
    arn              = "arn:aws:ssm:us-east-1:000000000000:parameter/datacenter-ssm-parameter"
    id               = "datacenter-ssm-parameter"
    name             = "datacenter-ssm-parameter"
    type             = "String"
    value            = "datacenter-value"
    version          = 1
}
```

### Step 3: Verify Parameter in AWS Console (Optional)

```bash
# List SSM parameters using AWS CLI
aws ssm describe-parameters --query 'Parameters[?Name==`datacenter-ssm-parameter`]'
```

**Expected JSON Output:**
```json
[
    {
        "Name": "datacenter-ssm-parameter",
        "Type": "String",
        "LastModifiedDate": "2025-09-29T23:14:00+05:00",
        "Version": 1,
        "Tier": "Standard",
        "DataType": "text"
    }
]
```

### Step 4: Retrieve Parameter Value via AWS CLI

```bash
# Get parameter value
aws ssm get-parameter --name datacenter-ssm-parameter --region us-east-1
```

**Expected JSON Output:**
```json
{
    "Parameter": {
        "Name": "datacenter-ssm-parameter",
        "Type": "String",
        "Value": "datacenter-value",
        "Version": 1,
        "LastModifiedDate": "2025-09-29T23:14:00+05:00",
        "ARN": "arn:aws:ssm:us-east-1:000000000000:parameter/datacenter-ssm-parameter",
        "DataType": "text"
    }
}
```

---

## 🧪 Testing

### Verify Parameter Creation

```bash
# Show complete parameter details from Terraform state
terraform state show aws_ssm_parameter.datacenter_ssm_parameter
```

### Test Parameter Retrieval via AWS CLI

```bash
# Retrieve parameter value to confirm accessibility
aws ssm get-parameter --name datacenter-ssm-parameter --region us-east-1 --query 'Parameter.Value' --output text
```

**Expected Output:**
```
datacenter-value
```

### Test Parameter Accessibility (Optional)

```bash
# List parameters to verify existence
aws ssm describe-parameters --region us-east-1 --query 'Parameters[?Name==`datacenter-ssm-parameter`].{Name:Name,Type:Type}'
```

**Expected Output:**
```json
[
    {
        "Name": "datacenter-ssm-parameter",
        "Type": "String"
    }
]
```

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure AWS provider for us-east-1 region
provider "aws" {
  region = "us-east-1"
}

# Create SSM parameter for storing configuration data
resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
terraform plan
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list

# Verify via AWS CLI
aws ssm get-parameter --name datacenter-ssm-parameter --region us-east-1
```

### Optional: Enhanced Configuration

```hcl
# Enhanced SSM parameter configuration with additional features
provider "aws" {
  region = "us-east-1"
}

resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"

  tags = {
    Name        = "datacenter-ssm-parameter"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "configuration-storage"
  }
}

# Optional: Add outputs for easy reference
output "parameter_name" {
  description = "Name of the created SSM parameter"
  value       = aws_ssm_parameter.datacenter_ssm_parameter.name
}

output "parameter_arn" {
  description = "ARN of the created SSM parameter"
  value       = aws_ssm_parameter.datacenter_ssm_parameter.arn
}

output "parameter_value" {
  description = "Value of the created SSM parameter"
  value       = aws_ssm_parameter.datacenter_ssm_parameter.value
  sensitive   = true
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Parameter Already Exists**

- **Symptoms:** Error: "ParameterAlreadyExists: Parameter datacenter-ssm-parameter already exists"
- **Solution:** Import the existing parameter or use a different name
```bash
# Import existing parameter into Terraform state
terraform import aws_ssm_parameter.datacenter_ssm_parameter datacenter-ssm-parameter
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDeniedException" or "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have SSM permissions
```bash
# Required permissions:
# - ssm:PutParameter
# - ssm:GetParameter
# - ssm:DescribeParameters
# - ssm:TagResource (if using tags)
```

**Issue 3: Invalid Parameter Name**

- **Symptoms:** Error: "InvalidParameter: Invalid parameter name"
- **Solution:** Ensure parameter name meets AWS SSM naming requirements
```bash
# Valid characters: A-Z, a-z, 0-9, /, ., -, _
# Maximum length: 2048 characters
# Must not start with "aws" or "ssm" (case-insensitive)
```

**Issue 4: Region Mismatch**

- **Symptoms:** Parameter not found when querying with AWS CLI
- **Solution:** Ensure the provider region is set to `us-east-1`
```bash
provider "aws" {
  region = "us-east-1"
}
```

**Issue 5: Terraform Plan Shows Changes**

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

- **🔐 Secure Storage:** Using SSM Parameter Store for centralized configuration management.
- **📊 Resource Naming:** Clear, descriptive parameter name (`datacenter-ssm-parameter`) matching requirements.
- **🏷️ Minimal Configuration:** Focused setup with only required attributes for simplicity.
- **🔄 Region Specificity:** Explicitly set `us-east-1` to meet requirements.
- **📍 Verifiability:** Ensured parameter can be retrieved via AWS CLI and Terraform.

---

## 🚀 Production Considerations

### SSM Parameter Security Framework
- **Encryption:** Use `SecureString` type with KMS for sensitive data (not required here).
- **Access Control:** Restrict parameter access with IAM policies.
- **Monitoring:** Enable CloudTrail for auditing parameter access.
- **Versioning:** Leverage parameter versioning for change tracking.
- **Tagging:** Add tags for cost allocation and resource management.

### Enhanced Parameter Configuration

```hcl
# Production-ready SSM parameter with additional features
provider "aws" {
  region = "us-east-1"
}

resource "aws_ssm_parameter" "datacenter_ssm_parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"

  description = "Configuration parameter for datacenter applications"

  tags = {
    Name        = "datacenter-ssm-parameter"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "configuration-storage"
  }
}
```

### Configuration Management Strategy
- **Access Policies:** Restrict read/write access with IAM policies.
- **Version Control:** Use versioning to track parameter changes.
- **Monitoring:** Enable CloudTrail for security auditing.
- **Hierarchy:** Organize parameters using paths (e.g., `/datacenter/config/`).
- **Cost Optimization:** Monitor parameter usage for cost efficiency.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **SSM Parameter Store Fundamentals:** Understanding centralized configuration management.
- **Parameter Configuration:** Setting up a String parameter with a specific value.
- **Region-Specific Deployment:** Targeting `us-east-1` for regional resources.
- **Terraform Integration:** Managing AWS SSM resources with Terraform.

**Terraform Features Used:**
- `aws_ssm_parameter`: SSM parameter resource creation.
- Provider configuration for region-specific deployment.
- AWS provider integration for configuration management.
- Verification via Terraform state and AWS CLI.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Parameter Name**: Created `datacenter-ssm-parameter` as required.
- **Parameter Type**: Set to `String`.
- **Parameter Value**: Configured as `datacenter-value`.
- **Region**: Deployed in `us-east-1`.
- **File Structure**: Used single `main.tf` file as specified.
- **Verification**: Confirmed parameter creation and retrieval via Terraform and AWS CLI.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Configuration Foundation:** The SSM parameter `datacenter-ssm-parameter` is now created in `us-east-1` and ready for use in application configuration, providing a secure and centralized storage solution for the Nautilus DevOps team.

### 🔮 Parameter Ready for:
- **Configuration Retrieval**: Access by applications via AWS SDK or CLI.
- **Security Enhancements**: Upgrade to `SecureString` for sensitive data.
- **Access Management**: Restrict access with IAM policies.
- **Monitoring**: Track access with CloudTrail.
- **Scalability**: Support additional parameters in a hierarchical structure.