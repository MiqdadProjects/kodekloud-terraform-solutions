# 🌟 Task 17 - Create DynamoDB Table Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** needs to set up a DynamoDB table for storing user data in the AWS cloud. They require a DynamoDB table with specific configurations to manage datacenter user information effectively. The table will serve as a scalable, serverless database for the team's applications.

**Requirements:**
- Create a DynamoDB table named **`datacenter-users`** using Terraform.
- The primary key should be **`datacenter_id`** with type **String**.
- The table should use the **PAY_PER_REQUEST** billing mode.
- The Terraform working directory is **`/home/bob/terraform`**.
- Create the **`main.tf`** file (do not create a different `.tf` file).

👉 **Your task:** Create a DynamoDB table using Terraform to meet the specified requirements, establishing a foundation for storing user data in the AWS environment.

💡 **Note:** DynamoDB tables provide a fully managed NoSQL database service for scalable and high-performance data storage.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (DynamoDB, region-specific)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- DynamoDB table for user data storage
- Primary key for unique identification
- Pay-per-request billing for cost efficiency
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **DynamoDB Table Resource:** Creates a NoSQL table for storing user data.
- **Primary Key:** Uses `datacenter_id` (String) for unique record identification.
- **Billing Mode:** Configures pay-per-request billing to optimize costs.
- **Scalability Foundation:** Prepares the table for dynamic workload handling.

### 🎯 Implementation Strategy
1. Create a DynamoDB table with the specified name (`datacenter-users`).
2. Configure the primary key as `datacenter_id` with String type.
3. Set the billing mode to `PAY_PER_REQUEST`.
4. Deploy and verify the table creation using Terraform.

---

## 🚀 Implementation Steps

### Step 1: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

---

### Step 2: Create Main Terraform Configuration

Create the `main.tf` file with the complete solution:

```hcl
# main.tf

# Create DynamoDB table for storing user data
resource "aws_dynamodb_table" "datacenter_users" {
  name           = "datacenter-users"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "datacenter_id"

  attribute {
    name = "datacenter_id"
    type = "S"
  }
}
```

**Configuration Breakdown:**
- `aws_dynamodb_table`: Defines a DynamoDB table resource.
- `name = "datacenter-users"`: Sets the table name as required.
- `billing_mode = "PAY_PER_REQUEST"`: Configures on-demand billing for cost efficiency.
- `hash_key = "datacenter_id"`: Specifies the primary key attribute.
- `attribute`: Defines the `datacenter_id` attribute with type `S` (String).

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

  # aws_dynamodb_table.datacenter_users will be created
  + resource "aws_dynamodb_table" "datacenter_users" {
      + arn              = (known after apply)
      + billing_mode     = "PAY_PER_REQUEST"
      + hash_key         = "datacenter_id"
      + id               = (known after apply)
      + name             = "datacenter-users"
      + tags_all         = (known after apply)
      + attribute {
          + name = "datacenter_id"
          + type = "S"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Create DynamoDB table for storing user data
resource "aws_dynamodb_table" "datacenter_users" {
  name           = "datacenter-users"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "datacenter_id"

  attribute {
    name = "datacenter_id"
    type = "S"
  }
}
```

### DynamoDB Table Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **name** | datacenter-users | The name of the DynamoDB table |
| **billing_mode** | PAY_PER_REQUEST | On-demand billing mode for automatic scaling |
| **hash_key** | datacenter_id | The primary key attribute for the table |
| **attribute.name** | datacenter_id | Name of the primary key attribute |
| **attribute.type** | S | String type for the primary key |
| **read_capacity** | null | Not specified (managed by PAY_PER_REQUEST) |
| **write_capacity** | null | Not specified (managed by PAY_PER_REQUEST) |

### DynamoDB Table Default Properties
- **Partition Key:** `datacenter_id` (String) as the primary key.
- **Billing Mode:** `PAY_PER_REQUEST` eliminates the need for manual capacity planning.
- **Indexes:** No secondary indexes defined (not required).
- **Encryption:** Server-side encryption enabled by default.
- **Tags:** No tags defined (optional for future use).

---

## ✅ Verification Steps

### Step 1: Verify Table Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_dynamodb_table.datacenter_users
```

### Step 2: Check Table Details in Terraform State

```bash
# Show detailed table information from Terraform state
terraform state show aws_dynamodb_table.datacenter_users
```

**Expected Output:**
```
# aws_dynamodb_table.datacenter_users:
resource "aws_dynamodb_table" "datacenter_users" {
    arn              = "arn:aws:dynamodb:us-west-2:123456789012:table/datacenter-users"
    billing_mode     = "PAY_PER_REQUEST"
    hash_key         = "datacenter_id"
    id               = "datacenter-users"
    name             = "datacenter-users"
    tags_all         = {}
    attribute {
        name = "datacenter_id"
        type = "S"
    }
}
```

### Step 3: Verify Table in AWS Console (Optional)

```bash
# List DynamoDB tables using AWS CLI
aws dynamodb list-tables --query 'TableNames[?@==`datacenter-users`]'
```

**Expected JSON Output:**
```json
["datacenter-users"]
```

### Step 4: Check Table Details via AWS CLI

```bash
# Get specific table details
aws dynamodb describe-table --table-name datacenter-users
```

**Expected JSON Output (partial):**
```json
{
    "Table": {
        "TableName": "datacenter-users",
        "TableStatus": "ACTIVE",
        "BillingModeSummary": {
            "BillingMode": "PAY_PER_REQUEST"
        },
        "AttributeDefinitions": [
            {
                "AttributeName": "datacenter_id",
                "AttributeType": "S"
            }
        ],
        "KeySchema": [
            {
                "AttributeName": "datacenter_id",
                "KeyType": "HASH"
            }
        ],
        "TableArn": "arn:aws:dynamodb:us-west-2:123456789012:table/datacenter-users",
        "CreationDateTime": "2025-09-29T22:57:00+05:00"
    }
}
```

---

## 🧪 Testing

### Verify Table Creation

```bash
# Show complete table details from Terraform state
terraform state show aws_dynamodb_table.datacenter_users
```

### Test Table Existence via AWS CLI

```bash
# Describe the table to confirm its configuration
aws dynamodb describe-table --table-name datacenter-users
```

### Test Table Accessibility (Optional)

```bash
# Attempt a simple scan to verify table functionality
aws dynamodb scan --table-name datacenter-users
```

**Expected Output (empty table):**
```json
{
    "Items": [],
    "Count": 0,
    "ScannedCount": 0
}
```

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Create DynamoDB table for storing user data
resource "aws_dynamodb_table" "datacenter_users" {
  name           = "datacenter-users"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "datacenter_id"

  attribute {
    name = "datacenter_id"
    type = "S"
  }
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
aws dynamodb describe-table --table-name datacenter-users
```

### Optional: Enhanced Configuration

```hcl
# Enhanced DynamoDB table configuration with tags
resource "aws_dynamodb_table" "datacenter_users" {
  name           = "datacenter-users"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "datacenter_id"

  attribute {
    name = "datacenter_id"
    type = "S"
  }

  tags = {
    Name        = "datacenter-users"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "user-data-storage"
  }
}

# Optional: Add outputs for easy reference
output "table_name" {
  description = "Name of the created DynamoDB table"
  value       = aws_dynamodb_table.datacenter_users.name
}

output "table_arn" {
  description = "ARN of the created DynamoDB table"
  value       = aws_dynamodb_table.datacenter_users.arn
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Table Already Exists**

- **Symptoms:** Error: "ResourceInUseException: Table already exists: datacenter-users"
- **Solution:** Import the existing table or use a different name
```bash
# Import existing table into Terraform state
terraform import aws_dynamodb_table.datacenter_users datacenter-users
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDeniedException" or "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have DynamoDB permissions
```bash
# Required permissions:
# - dynamodb:CreateTable
# - dynamodb:DescribeTable
# - dynamodb:ListTables
# - dynamodb:TagResource (if using tags)
```

**Issue 3: Invalid Attribute Type**

- **Symptoms:** Error: "ValidationException: Invalid attribute type"
- **Solution:** Ensure attribute type is `S` for String
```bash
# Verify attribute block
attribute {
  name = "datacenter_id"
  type = "S"
}
```

**Issue 4: Billing Mode Misconfiguration**

- **Symptoms:** Error during `terraform apply` due to incorrect billing mode
- **Solution:** Confirm `billing_mode` is set to `PAY_PER_REQUEST`
```bash
billing_mode = "PAY_PER_REQUEST"
```

---

## 💡 Best Practices Applied

- **🔐 Scalability:** Using `PAY_PER_REQUEST` ensures automatic scaling without manual capacity management.
- **📊 Resource Naming:** Clear, descriptive table name (`datacenter-users`) matching requirements.
- **🏷️ Minimal Configuration:** Focused setup with only required attributes for simplicity.
- **🔄 Cost Efficiency:** Pay-per-request billing optimizes costs for variable workloads.
- **📍 Data Integrity:** Primary key ensures unique identification of records.

---

## 🚀 Production Considerations

### DynamoDB Table Security Framework
- **Encryption:** Server-side encryption is enabled by default for data protection.
- **Access Control:** Attach IAM policies to restrict table access to authorized users.
- **Monitoring:** Enable CloudWatch metrics for performance tracking.
- **Backups:** Configure point-in-time recovery for data protection.
- **Tagging:** Add tags for cost allocation and resource management.

### Enhanced Table Configuration

```hcl
# Production-ready DynamoDB table with additional features
resource "aws_dynamodb_table" "datacenter_users" {
  name           = "datacenter-users"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "datacenter_id"

  attribute {
    name = "datacenter_id"
    type = "S"
  }

  server_side_encryption {
    enabled = true
  }

  point_in_time_recovery {
    enabled = true
  }

  tags = {
    Name        = "datacenter-users"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "user-data-storage"
  }
}
```

### Data Management Strategy
- **Indexes:** Add global or local secondary indexes for complex queries (if needed).
- **Access Policies:** Use IAM policies to control read/write access.
- **Monitoring:** Enable CloudWatch alarms for usage spikes.
- **Backup Strategy:** Schedule regular backups for disaster recovery.
- **Cost Optimization:** Monitor usage to ensure pay-per-request remains cost-effective.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **DynamoDB Fundamentals:** Understanding NoSQL table creation and configuration.
- **Primary Key Design:** Using a partition key for unique record identification.
- **Billing Modes:** Implementing cost-efficient pay-per-request billing.
- **Terraform Integration:** Managing AWS DynamoDB resources with Terraform.

**Terraform Features Used:**
- `aws_dynamodb_table`: DynamoDB table resource creation.
- Attribute definitions for primary key configuration.
- Pay-per-request billing mode setup.
- AWS provider integration for NoSQL database management.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Table Name**: Created `datacenter-users` as required.
- **Primary Key**: Configured `datacenter_id` (String) as the partition key.
- **Billing Mode**: Set to `PAY_PER_REQUEST` for scalability and cost efficiency.
- **File Structure**: Used single `main.tf` file as specified.
- **Verification**: Confirmed table creation through Terraform state and AWS CLI.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Data Storage Foundation:** The DynamoDB table `datacenter-users` is now created and ready for storing user data, providing a scalable and cost-efficient solution for the Nautilus DevOps team’s application needs.

### 🔮 Table Ready for:
- **Data Storage**: Store user data with `datacenter_id` as the unique identifier.
- **Access Management**: Attach IAM policies for secure access.
- **Scalability**: Handle variable workloads with pay-per-request billing.
- **Backups**: Enable point-in-time recovery for data protection.
- **Monitoring**: Integrate with CloudWatch for performance tracking.