```markdown
# 🌟 Task 13 - Create Private S3 Bucket Using Terraform

## 📌 Task Description

As part of the data migration process, the **Nautilus DevOps team** is actively creating several S3 buckets on AWS using Terraform. They plan to utilize both private and public S3 buckets to store the relevant data. Given the ongoing migration of other infrastructure to AWS, it is logical to consolidate data storage within the AWS environment as well.

This task focuses on creating a private S3 bucket with comprehensive security controls to ensure sensitive data remains protected and inaccessible to unauthorized public access.

**Requirements:**
- The name of the S3 bucket must be **`nautilus-s3-15729`**
- The S3 bucket must **block all public access**, making it a private bucket
- Use Terraform to provision the S3 bucket
- Ensure the resources are created in the **us-east-1** region
- The bucket must have block public access enabled to restrict any public access
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create a secure, private S3 bucket with comprehensive public access restrictions using Terraform.

💡 **Note:** Private S3 buckets with blocked public access are essential for storing sensitive data and maintaining security compliance.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- Private S3 Bucket with comprehensive access restrictions
- Public Access Block configuration for maximum security
- Secure data storage environment

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **S3 Bucket Resource:** Creates the main private storage bucket
- **Public Access Block:** Enforces comprehensive public access restrictions
- **Security Configuration:** All four public access block settings enabled
- **Private Data Environment:** Secure storage for sensitive information

### 🎯 Implementation Strategy
1. Create S3 bucket with the specified name (nautilus-s3-15729)
2. Configure comprehensive public access block settings
3. Ensure all public access vectors are blocked
4. Maintain private bucket security posture

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

# Create private S3 bucket for secure data storage
resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-15729"
}

# Block all public access to ensure private bucket
resource "aws_s3_bucket_public_access_block" "nautilus_block" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

**Configuration Breakdown:**
- `aws_s3_bucket` - Creates the S3 bucket with specified name
- `aws_s3_bucket_public_access_block` - Blocks all forms of public access
- `block_public_acls = true` - Prevents public ACL configurations
- `block_public_policy = true` - Blocks public bucket policies
- `ignore_public_acls = true` - Ignores any existing public ACLs
- `restrict_public_buckets = true` - Restricts public bucket access

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

  # aws_s3_bucket.nautilus_bucket will be created
  + resource "aws_s3_bucket" "nautilus_bucket" {
      + acceleration_status         = (known after apply)
      + acl                        = (known after apply)
      + arn                        = (known after apply)
      + bucket                     = "nautilus-s3-15729"
      + bucket_domain_name         = (known after apply)
      + bucket_prefix              = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy              = false
      + hosted_zone_id             = (known after apply)
      + id                         = (known after apply)
      + object_lock_enabled        = (known after apply)
      + policy                     = (known after apply)
      + region                     = (known after apply)
      + request_payer              = (known after apply)
      + tags_all                   = (known after apply)
      + website_domain             = (known after apply)
      + website_endpoint           = (known after apply)
    }

  # aws_s3_bucket_public_access_block.nautilus_block will be created
  + resource "aws_s3_bucket_public_access_block" "nautilus_block" {
      + block_public_acls       = true
      + block_public_policy     = true
      + bucket                  = (known after apply)
      + id                      = (known after apply)
      + ignore_public_acls      = true
      + restrict_public_buckets = true
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create private S3 bucket for secure data storage
resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-15729"              # Required bucket name
}

# Block all public access to ensure private bucket
resource "aws_s3_bucket_public_access_block" "nautilus_block" {
  bucket = aws_s3_bucket.nautilus_bucket.id # Reference to created bucket
  
  block_public_acls       = true            # Block public ACL configurations
  block_public_policy     = true            # Block public bucket policies
  ignore_public_acls      = true            # Ignore existing public ACLs
  restrict_public_buckets = true            # Restrict all public bucket access
}
```

### Public Access Block Settings Explained
| Setting | Value | Security Impact |
|---------|--------|----------------|
| **block_public_acls** | true | Prevents new public ACLs and modification of existing ones |
| **block_public_policy** | true | Blocks public bucket policies and access point policies |
| **ignore_public_acls** | true | Ignores all public ACLs on bucket and objects |
| **restrict_public_buckets** | true | Restricts access to buckets with public policies |

### Security Posture Comparison
| Configuration | Public Bucket | Private Bucket |
|---------------|---------------|----------------|
| block_public_acls | false | **true** |
| block_public_policy | false | **true** |
| ignore_public_acls | false | **true** |
| restrict_public_buckets | false | **true** |
| **Result** | Public Access | **Private & Secure** |

---

## ✅ Verification Steps

### Step 1: Verify Resources Created
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_s3_bucket.nautilus_bucket
aws_s3_bucket_public_access_block.nautilus_block
```

### Step 2: Check Bucket Configuration
```bash
# Show bucket details from Terraform state
terraform state show aws_s3_bucket.nautilus_bucket
```

### Step 3: Verify Public Access Block Settings
```bash
# Check public access block configuration
aws s3api get-public-access-block --bucket nautilus-s3-15729
```

**Expected Output:**
```json
{
    "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "IgnorePublicAcls": true,
        "BlockPublicPolicy": true,
        "RestrictPublicBuckets": true
    }
}
```

---

## 🧪 Testing

### Verify Private Bucket Configuration
```bash
# Show complete bucket configuration from Terraform state
terraform state show aws_s3_bucket_public_access_block.nautilus_block
```

### AWS CLI Verification
```bash
# List S3 buckets to confirm creation
aws s3 ls | grep nautilus-s3-15729
```

**Expected Output:**
```
2024-XX-XX XX:XX:XX nautilus-s3-15729
```

### Test Private Access (Security Verification)
```bash
# Attempt to set public ACL (should fail with private bucket)
aws s3api put-bucket-acl --bucket nautilus-s3-15729 --acl public-read
```

**Expected Error:**
```
An error occurred (AccessDenied) when calling the PutBucketAcl operation: Access Denied
```

### Verify Bucket Location and Region
```bash
# Confirm bucket is in us-east-1 region
aws s3api get-bucket-location --bucket nautilus-s3-15729
```

**Expected Output:**
```json
{
    "LocationConstraint": null
}
```
*Note: null indicates us-east-1 (default region)*

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create private S3 bucket for secure data storage
resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-15729"
}

# Block all public access to ensure private bucket
resource "aws_s3_bucket_public_access_block" "nautilus_block" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
terraform state list
terraform state show aws_s3_bucket.nautilus_bucket
```

### Optional: Enhanced Configuration with Additional Security
```hcl
# Enhanced private bucket with additional security features
resource "aws_s3_bucket" "nautilus_bucket" {
  bucket = "nautilus-s3-15729"
}

resource "aws_s3_bucket_public_access_block" "nautilus_block" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

# Optional: Add server-side encryption
resource "aws_s3_bucket_server_side_encryption_configuration" "encryption" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

# Optional: Add versioning for data protection
resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  versioning_configuration {
    status = "Enabled"
  }
}
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient bucket information
output "bucket_name" {
  description = "Name of the private S3 bucket"
  value       = aws_s3_bucket.nautilus_bucket.id
}

output "bucket_arn" {
  description = "ARN of the private S3 bucket"
  value       = aws_s3_bucket.nautilus_bucket.arn
}

output "public_access_block_status" {
  description = "Public access block configuration"
  value = {
    block_public_acls       = aws_s3_bucket_public_access_block.nautilus_block.block_public_acls
    block_public_policy     = aws_s3_bucket_public_access_block.nautilus_block.block_public_policy
    ignore_public_acls      = aws_s3_bucket_public_access_block.nautilus_block.ignore_public_acls
    restrict_public_buckets = aws_s3_bucket_public_access_block.nautilus_block.restrict_public_buckets
  }
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Bucket name already exists**
- **Symptoms:** Error: "BucketAlreadyExists"
- **Solution:** S3 bucket names must be globally unique across all AWS accounts
```bash
# If bucket exists, either use a different name or import existing bucket
terraform import aws_s3_bucket.nautilus_bucket nautilus-s3-15729
```

**Issue 2: Public access block conflicts**
- **Symptoms:** Error applying public access block settings
- **Solution:** Ensure bucket is created first before applying access blocks
```bash
# Terraform handles dependencies automatically, but manual intervention may be needed
terraform apply -target=aws_s3_bucket.nautilus_bucket
terraform apply
```

**Issue 3: Region specification issues**
- **Symptoms:** Bucket created in wrong region
- **Solution:** Ensure AWS provider is configured for us-east-1
```hcl
provider "aws" {
  region = "us-east-1"
}
```

**Issue 4: Permission errors**
- **Symptoms:** "Access Denied" errors during bucket creation
- **Solution:** Verify AWS credentials have S3 permissions
```bash
# Required permissions:
# - s3:CreateBucket
# - s3:GetBucketPublicAccessBlock
# - s3:PutBucketPublicAccessBlock
# - s3:ListBucket
```

---

## 💡 Best Practices Applied

- **🔐 Maximum Security:** All four public access block settings enabled
- **📊 Resource Management:** Clean resource dependencies and references
- **🏷️ Naming Conventions:** Consistent naming matching requirements
- **🔄 Private Configuration:** Comprehensive private access enforcement
- **📍 Regional Compliance:** Explicit deployment in us-east-1 region

---

## 🚀 Production Considerations

### Enhanced Security Configuration
- **Encryption:** Implement server-side encryption for data at rest
- **Access Logging:** Enable S3 access logging for audit trails
- **MFA Delete:** Require MFA for object deletion in versioned buckets
- **Cross-Region Replication:** Set up replication for disaster recovery
- **IAM Policies:** Implement least-privilege access policies

### Private Bucket Security Framework
```hcl
# Production-grade private bucket configuration
resource "aws_s3_bucket_server_side_encryption_configuration" "encryption" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  rule {
    apply_server_side_encryption_by_default {
      kms_master_key_id = aws_kms_key.s3_key.arn
      sse_algorithm     = "aws:kms"
    }
    bucket_key_enabled = true
  }
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  versioning_configuration {
    status     = "Enabled"
    mfa_delete = "Enabled"
  }
}

resource "aws_s3_bucket_notification" "audit_notifications" {
  bucket = aws_s3_bucket.nautilus_bucket.id
  
  cloudwatch_configuration {
    cloudwatch_configuration_id = "audit-events"
    events                      = ["s3:ObjectCreated:*", "s3:ObjectRemoved:*"]
  }
}
```

### Monitoring and Compliance
- **CloudTrail:** Enable S3 API logging through CloudTrail
- **Config Rules:** Set up AWS Config rules for compliance monitoring
- **GuardDuty:** Enable GuardDuty for threat detection
- **Access Patterns:** Monitor and analyze S3 access patterns

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Private S3 Bucket Security:** Comprehensive public access restriction
- **Public Access Block:** Understanding and implementing all four security settings
- **Data Security:** Creating secure storage environments for sensitive data

**Terraform Features Used:**
- `aws_s3_bucket` - S3 bucket resource creation
- `aws_s3_bucket_public_access_block` - Public access restriction management
- Resource dependencies and security configuration
- Private bucket security implementation

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Bucket Name** - Created "nautilus-s3-15729" as required
- **Private Configuration** - Blocked all public access vectors
- **Security Settings** - Enabled all four public access block restrictions
- **Regional Deployment** - Created in us-east-1 region as specified
- **File Structure** - Used single `main.tf` file as required

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Security Foundation:** The private S3 bucket is now fully secured with comprehensive public access restrictions, providing a safe environment for storing sensitive data as part of the Nautilus data migration strategy.

### 🔮 Private Bucket Ready for:
- **Sensitive Data Storage:** Secure storage for confidential information
- **Internal Applications:** Backend storage for internal systems
- **Compliance Requirements:** Meeting regulatory data protection standards
- **Backup Storage:** Secure backup destination for critical data
- **Development Environments:** Safe storage for development and testing data
```