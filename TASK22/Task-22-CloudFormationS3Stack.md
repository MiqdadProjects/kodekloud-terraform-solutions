# 🌟 Task 22 - Create AWS CloudFormation Stack with S3 Bucket Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is automating infrastructure deployment using AWS CloudFormation. They need to create a CloudFormation stack that provisions an S3 bucket with versioning enabled to support secure storage for application assets.

**Requirements:**
- Create a CloudFormation stack named **`nautilus-stack`** using Terraform.
- The stack should provision an S3 bucket named **`nautilus-bucket-31516`** with versioning enabled.
- The Terraform working directory is **`/home/bob/terraform`**.
- Create the **`main.tf`** file (do not create a different `.tf` file).

👉 **Your task:** Use Terraform to create a CloudFormation stack that provisions an S3 bucket with versioning enabled, ensuring the infrastructure is deployed and verifiable via Terraform and AWS CLI.

💡 **Note:** AWS CloudFormation is a service for provisioning AWS resources using templates. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable via Terraform and AWS CLI. The current date and time is September 30, 2025, 09:56 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (CloudFormation, S3, region-specific)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- CloudFormation stack to manage infrastructure
- S3 bucket with versioning enabled
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **CloudFormation Stack Resource:** Defines a stack named `nautilus-stack` to manage resources.
- **S3 Bucket Resource (within CloudFormation):** Creates a bucket named `nautilus-bucket-31516` with versioning enabled.
- **Terraform Integration:** Uses Terraform to deploy the CloudFormation stack.
- **Storage Framework:** Prepares for secure, versioned storage via S3.

### 🎯 Implementation Strategy
1. Create a Terraform configuration in `main.tf` to define the `aws_cloudformation_stack` resource.
2. Embed a CloudFormation template within the Terraform configuration to provision the S3 bucket.
3. Set the bucket name to `nautilus-bucket-31516` and enable versioning.
4. Deploy the stack using Terraform in the specified directory.
5. Verify the stack and bucket creation using Terraform state and AWS CLI.

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

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Create CloudFormation stack to provision S3 bucket
resource "aws_cloudformation_stack" "nautilus_stack" {
  name = "nautilus-stack"

  template_body = <<EOT
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "CloudFormation stack to create S3 bucket with versioning enabled",
  "Resources": {
    "NautilusBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "nautilus-bucket-31516",
        "VersioningConfiguration": {
          "Status": "Enabled"
        }
      }
    }
  }
}
EOT
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (adjust if a different region is required).
- `aws_cloudformation_stack`: Defines a CloudFormation stack named `nautilus-stack`.
- `name = "nautilus-stack"`: Sets the stack name as required.
- `template_body`: Embeds a CloudFormation template in JSON format.
- `AWSTemplateFormatVersion`: Specifies the CloudFormation template version (`2010-09-09`).
- `Resources.NautilusBucket`: Defines an S3 bucket resource within the CloudFormation template.
- `BucketName = "nautilus-bucket-31516"`: Sets the bucket name as required.
- `VersioningConfiguration.Status = "Enabled"`: Enables versioning for the bucket.

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

  # aws_cloudformation_stack.nautilus_stack will be created
  + resource "aws_cloudformation_stack" "nautilus_stack" {
      + id            = (known after apply)
      + name          = "nautilus-stack"
      + outputs       = (known after apply)
      + template_body = jsonencode(
            {
              AWSTemplateFormatVersion = "2010-09-09"
              Description              = "CloudFormation stack to create S3 bucket with versioning enabled"
              Resources                = {
                  NautilusBucket = {
                      Properties = {
                          BucketName            = "nautilus-bucket-31516"
                          VersioningConfiguration = {
                              Status = "Enabled"
                          }
                      }
                      Type       = "AWS::S3::Bucket"
                  }
              }
            }
        )
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
aws_cloudformation_stack.nautilus_stack: Refreshing state... [id=arn:aws:cloudformation:us-east-1:000000000000:stack/nautilus-stack/...]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Create CloudFormation stack to provision S3 bucket
resource "aws_cloudformation_stack" "nautilus_stack" {
  name = "nautilus-stack"

  template_body = <<EOT
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "CloudFormation stack to create S3 bucket with versioning enabled",
  "Resources": {
    "NautilusBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "nautilus-bucket-31516",
        "VersioningConfiguration": {
          "Status": "Enabled"
        }
      }
    }
  }
}
EOT
}
```

### CloudFormation Stack and S3 Bucket Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **Stack: name** | nautilus-stack | Name of the CloudFormation stack |
| **Stack: template_body** | JSON template | Defines the S3 bucket resource |
| **S3 Bucket: BucketName** | nautilus-bucket-31516 | Unique name of the S3 bucket |
| **S3 Bucket: VersioningConfiguration.Status** | Enabled | Enables versioning for object retention |

### CloudFormation Stack and S3 Bucket Properties
- **Region:** `us-east-1` (specified in provider, adjust if needed).
- **Stack Outputs:** None defined (can be added for resource references).
- **Bucket Access:** Controlled by IAM permissions (public access not explicitly blocked in this template).
- **Dependency Management:** The S3 bucket is managed within the CloudFormation stack.

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_cloudformation_stack.nautilus_stack
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed stack information
terraform state show aws_cloudformation_stack.nautilus_stack
```

**Expected Output:**
```
# aws_cloudformation_stack.nautilus_stack:
resource "aws_cloudformation_stack" "nautilus_stack" {
    id            = "arn:aws:cloudformation:us-east-1:000000000000:stack/nautilus-stack/..."
    name          = "nautilus-stack"
    template_body = jsonencode(
        {
            AWSTemplateFormatVersion = "2010-09-09"
            Description              = "CloudFormation stack to create S3 bucket with versioning enabled"
            Resources                = {
                NautilusBucket = {
                    Properties = {
                        BucketName            = "nautilus-bucket-31516"
                        VersioningConfiguration = {
                            Status = "Enabled"
                        }
                    }
                    Type       = "AWS::S3::Bucket"
                }
            }
        }
    )
}
```

### Step 3: Verify Resources in AWS Console (Optional)

```bash
# List CloudFormation stacks using AWS CLI
aws cloudformation describe-stacks --stack-name nautilus-stack
```

**Expected JSON Output (partial):**
```json
{
    "Stacks": [
        {
            "StackName": "nautilus-stack",
            "CreationTime": "2025-09-30T21:56:00Z",
            "StackStatus": "CREATE_COMPLETE",
            "Description": "CloudFormation stack to create S3 bucket with versioning enabled"
        }
    ]
}
```

```bash
# List S3 buckets to confirm bucket creation
aws s3 ls
```

**Expected Output:**
```
2025-09-30 21:56:00 nautilus-bucket-31516
```

```bash
# Check versioning status of the bucket
aws s3api get-bucket-versioning --bucket nautilus-bucket-31516
```

**Expected JSON Output:**
```json
{
    "Status": "Enabled"
}
```

---

## 🧪 Testing

### Verify Resource Creation

```bash
# Show complete stack details from Terraform state
terraform state show aws_cloudformation_stack.nautilus_stack
```

### Test Bucket Accessibility (Optional)

```bash
# Upload a test file to the bucket (requires appropriate permissions)
echo "Test file" > test.txt
aws s3 cp test.txt s3://nautilus-bucket-31516/test.txt
```

**Expected Output:**
```
upload: ./test.txt to s3://nautilus-bucket-31516/test.txt
```

```bash
# List objects in the bucket
aws s3 ls s3://nautilus-bucket-31516/
```

**Expected Output:**
```
2025-09-30 21:56:00          9 test.txt
```

### Test Versioning

```bash
# Upload another version of the same file
echo "Updated test file" > test.txt
aws s3 cp test.txt s3://nautilus-bucket-31516/test.txt
```

```bash
# List object versions
aws s3api list-object-versions --bucket nautilus-bucket-31516
```

**Expected Output (partial):**
```json
{
    "Versions": [
        {
            "Key": "test.txt",
            "VersionId": "version_id_2",
            "IsLatest": true,
            "LastModified": "2025-09-30T21:56:00Z"
        },
        {
            "Key": "test.txt",
            "VersionId": "version_id_1",
            "IsLatest": false,
            "LastModified": "2025-09-30T21:56:00Z"
        }
    ]
}
```

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Create CloudFormation stack to provision S3 bucket
resource "aws_cloudformation_stack" "nautilus_stack" {
  name = "nautilus-stack"

  template_body = <<EOT
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "CloudFormation stack to create S3 bucket with versioning enabled",
  "Resources": {
    "NautilusBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "nautilus-bucket-31516",
        "VersioningConfiguration": {
          "Status": "Enabled"
        }
      }
    }
  }
}
EOT
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
terraform state show aws_cloudformation_stack.nautilus_stack

# Verify via AWS CLI
aws cloudformation describe-stacks --stack-name nautilus-stack
aws s3 ls
aws s3api get-bucket-versioning --bucket nautilus-bucket-31516
```

### Optional: Enhanced Configuration

```hcl
# Enhanced CloudFormation stack with additional S3 bucket configurations
provider "aws" {
  region = "us-east-1"
}

resource "aws_cloudformation_stack" "nautilus_stack" {
  name = "nautilus-stack"

  template_body = <<EOT
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "CloudFormation stack to create S3 bucket with versioning and public access block",
  "Resources": {
    "NautilusBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "nautilus-bucket-31516",
        "VersioningConfiguration": {
          "Status": "Enabled"
        },
        "PublicAccessBlockConfiguration": {
          "BlockPublicAcls": true,
          "IgnorePublicAcls": true,
          "BlockPublicPolicy": true,
          "RestrictPublicBuckets": true
        },
        "Tags": [
          {
            "Key": "Name",
            "Value": "nautilus-bucket-31516"
          },
          {
            "Key": "Team",
            "Value": "nautilus-devops"
          },
          {
            "Key": "Environment",
            "Value": "production"
          }
        ]
      }
    }
  },
  "Outputs": {
    "BucketArn": {
      "Description": "ARN of the S3 bucket",
      "Value": {"Fn::GetAtt": ["NautilusBucket", "Arn"]}
    }
  }
}
EOT
}

# Optional: Outputs for easy reference
output "stack_id" {
  description = "ID of the created CloudFormation stack"
  value       = aws_cloudformation_stack.nautilus_stack.id
}

output "bucket_arn" {
  description = "ARN of the created S3 bucket"
  value       = aws_cloudformation_stack.nautilus_stack.outputs["BucketArn"]
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Stack Already Exists**

- **Symptoms:** Error: "Stack [nautilus-stack] already exists"
- **Solution:** Import the existing stack or use a different stack name
```bash
# Import existing stack into Terraform state
terraform import aws_cloudformation_stack.nautilus_stack arn:aws:cloudformation:us-east-1:000000000000:stack/nautilus-stack/...
```

**Issue 2: Bucket Already Exists**

- **Symptoms:** Error: "BucketAlreadyExists: The requested bucket name is not available"
- **Solution:** Ensure the bucket name is unique or import the existing bucket into the CloudFormation stack
```bash
# Update the CloudFormation template to reference an existing bucket or use a unique name
```

**Issue 3: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: cloudformation:CreateStack"
- **Solution:** Ensure AWS credentials have CloudFormation and S3 permissions
```bash
# Required permissions:
# - cloudformation:CreateStack
# - cloudformation:DescribeStacks
# - s3:CreateBucket
# - s3:PutBucketVersioning
# - s3:ListBucket
```

**Issue 4: Invalid CloudFormation Template**

- **Symptoms:** Error: "Template format error: JSON not well-formed"
- **Solution:** Validate the CloudFormation template syntax
```bash
# Validate template using AWS CLI
aws cloudformation validate-template --template-body file://main.tf
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

- **🔐 Infrastructure as Code:** Used Terraform to manage CloudFormation for consistent deployments.
- **📊 Resource Naming:** Clear stack (`nautilus-stack`) and bucket (`nautilus-bucket-31516`) names.
- **🏷️ Minimal Configuration:** Focused setup with only required attributes for simplicity.
- **🔄 Dependency Management:** CloudFormation template correctly defines the S3 bucket.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### CloudFormation and S3 Security Framework
- **Public Access:** Add `PublicAccessBlockConfiguration` to block public access (included in enhanced configuration).
- **Encryption:** Enable server-side encryption (e.g., AES256 or KMS) for data security.
- **Access Control:** Restrict bucket access with IAM policies or bucket policies.
- **Monitoring:** Enable CloudWatch metrics for bucket activity tracking.
- **Tagging:** Add tags for cost allocation and resource management.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_cloudformation_stack" "nautilus_stack" {
  name = "nautilus-stack"

  template_body = <<EOT
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "CloudFormation stack to create S3 bucket with versioning and encryption",
  "Resources": {
    "NautilusBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "nautilus-bucket-31516",
        "VersioningConfiguration": {
          "Status": "Enabled"
        },
        "PublicAccessBlockConfiguration": {
          "BlockPublicAcls": true,
          "IgnorePublicAcls": true,
          "BlockPublicPolicy": true,
          "RestrictPublicBuckets": true
        },
        "BucketEncryption": {
          "ServerSideEncryptionConfiguration": [
            {
              "ServerSideEncryptionByDefault": {
                "SSEAlgorithm": "AES256"
              }
            }
          ]
        },
        "Tags": [
          {
            "Key": "Name",
            "Value": "nautilus-bucket-31516"
          },
          {
            "Key": "Team",
            "Value": "nautilus-devops"
          },
          {
            "Key": "Environment",
            "Value": "production"
          }
        ]
      }
    }
  },
  "Outputs": {
    "BucketArn": {
      "Description": "ARN of the S3 bucket",
      "Value": {"Fn::GetAtt": ["NautilusBucket", "Arn"]}
    }
  }
}
EOT
}

output "stack_id" {
  description = "ID of the created CloudFormation stack"
  value       = aws_cloudformation_stack.nautilus_stack.id
}

output "bucket_arn" {
  description = "ARN of the created S3 bucket"
  value       = aws_cloudformation_stack.nautilus_stack.outputs["BucketArn"]
}
```

### Storage Strategy
- **Lifecycle Policies:** Add lifecycle rules to transition old versions to cheaper storage (e.g., Glacier).
- **Access Policies:** Restrict access to specific IAM roles/users.
- **Monitoring:** Set up CloudWatch alarms for bucket size or access anomalies.
- **Cost Optimization:** Monitor storage costs and use lifecycle rules to reduce expenses.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **CloudFormation Fundamentals:** Understanding stack creation and template structure.
- **S3 Integration:** Provisioning an S3 bucket with versioning via CloudFormation.
- **Terraform Integration:** Managing CloudFormation stacks with Terraform.
- **Dependency Management:** Ensuring proper resource dependencies in Terraform and CloudFormation.

**Terraform Features Used:**
- `aws_cloudformation_stack`: CloudFormation stack resource creation.
- Embedded CloudFormation template for S3 bucket provisioning.
- AWS provider integration for CloudFormation and S3 services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Stack Name**: Created `nautilus-stack` as required.
- **Bucket Name**: Provisioned `nautilus-bucket-31516` with versioning enabled.
- **File Structure**: Used single `main.tf` file as specified.
- **Verification**: Confirmed stack and bucket creation via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Storage Foundation:** The CloudFormation stack `nautilus-stack` and S3 bucket `nautilus-bucket-31516` are created with versioning enabled, ready for the Nautilus DevOps team to store application assets securely.

### 🔮 Resources Ready for:
- **Storage**: Store application assets and backups in the S3 bucket.
- **Recovery**: Use versioning to recover previous object versions.
- **Security**: Implement additional IAM policies or encryption.
- **Cost Management**: Configure lifecycle rules for cost optimization.
- **Integration**: Connect with applications for file uploads/downloads.