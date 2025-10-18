# 🌟 Task 28 - Enable Versioning for AWS S3 Bucket Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** requires data protection and recovery measures for one of their S3 buckets. To ensure data can be recovered in case of accidental deletion or corruption, versioning must be enabled for the specified S3 bucket.

**Requirements:**
- Enable versioning for the S3 bucket named **`nautilus-s3-5581`** using Terraform.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update the **`main.tf`** file (do not create a separate `.tf` file).
- Ensure the configuration is verifiable via Terraform and AWS CLI.

💡 **Note:** AWS S3 versioning allows multiple versions of an object to be stored, enabling recovery from accidental deletions or overwrites. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable. The current date and time is October 18, 2025, 05:26 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (S3, region-agnostic)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- S3 bucket named `nautilus-s3-5581`
- S3 bucket versioning configuration
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **S3 Bucket Resource:** Manages the existing `nautilus-s3-5581` bucket.
- **S3 Versioning Resource:** Enables versioning for the bucket.
- **Security Framework:** Ensures the bucket remains private with appropriate access controls.

### 🎯 Implementation Strategy
1. Identify the existing `nautilus-s3-5581` bucket using its name.
2. Update the `main.tf` file to include the `aws_s3_bucket` and `aws_s3_bucket_versioning` resources.
3. Import the existing bucket into the Terraform state to manage it.
4. Deploy the updated configuration to enable versioning.
5. Verify versioning status using Terraform state and AWS CLI.

---

## 🚀 Implementation Steps

### Step 1: Identify Existing Resources

Since the task implies the `nautilus-s3-5581` bucket may already exist, retrieve its details using AWS CLI.

```bash
# Get the S3 bucket details
aws s3api get-bucket-location --bucket nautilus-s3-5581
```

**Example Output:**
```json
{
    "LocationConstraint": "us-east-1"
}
```

```bash
# Check current versioning status
aws s3api get-bucket-versioning --bucket nautilus-s3-5581
```

**Example Output (if versioning is not enabled):**
```json
{}
```

**Note:** If the bucket does not exist, Terraform will create it. If it exists, it must be imported into the Terraform state.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Update Main Terraform Configuration

Update the `main.tf` file to manage the S3 bucket and enable versioning:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"  # S3 is global, but provider requires a region
}

# Manage S3 bucket
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-5581"

  tags = {
    Name = "nautilus-s3-5581"
  }
}

# Enable versioning for the S3 bucket
resource "aws_s3_bucket_versioning" "s3_ran_bucket_versioning" {
  bucket = aws_s3_bucket.s3_ran_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (S3 is global, but a region is required for the provider).
- `aws_s3_bucket`: Manages the `nautilus-s3-5581` bucket with a private ACL (default behavior in Terraform AWS provider v4+).
- `aws_s3_bucket_versioning`: Enables versioning for the bucket using the `versioning_configuration` block.
- `tags`: Applies the required name (`nautilus-s3-5581`) to the bucket via tags.

**Note:** The `acl` attribute is deprecated in newer Terraform AWS provider versions. The bucket is private by default, aligning with the provided solution's `acl = "private"`.

---

### Step 4: Import Existing Bucket (if it exists)

If the `nautilus-s3-5581` bucket already exists, import it into the Terraform state.

```bash
# Import the S3 bucket
terraform import aws_s3_bucket.s3_ran_bucket nautilus-s3-5581
```

**Note:** If the bucket does not exist, Terraform will create it during the `apply` step, so this step is optional.

---

### Step 5: Initialize Terraform (if not already initialized)

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

### Step 6: Format and Validate Configuration

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

### Step 7: Plan and Apply Configuration

```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply -auto-approve
```

**Expected Output:**
```
Terraform will perform the following actions:

  # aws_s3_bucket.s3_ran_bucket will be created
  + resource "aws_s3_bucket" "s3_ran_bucket" {
      + bucket        = "nautilus-s3-5581"
      + id            = (known after apply)
      + tags          = {
          + "Name" = "nautilus-s3-5581"
        }
    }

  # aws_s3_bucket_versioning.s3_ran_bucket_versioning will be created
  + resource "aws_s3_bucket_versioning" "s3_ran_bucket_versioning" {
      + bucket  = (known after apply)
      + id      = (known after apply)
      + versioning_configuration {
          + status = "Enabled"
        }
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

**Note:** If the bucket was imported, the output will show fewer resources being created.

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_s3_bucket.s3_ran_bucket: Refreshing state... [id=nautilus-s3-5581]
aws_s3_bucket_versioning.s3_ran_bucket_versioning: Refreshing state... [id=nautilus-s3-5581]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

### Step 9: Verify S3 Versioning

```bash
# Check versioning status using AWS CLI
aws s3api get-bucket-versioning --bucket nautilus-s3-5581
```

**Expected Output:**
```json
{
    "Status": "Enabled"
}
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage S3 bucket
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-5581"

  tags = {
    Name = "nautilus-s3-5581"
  }
}

# Enable versioning for the S3 bucket
resource "aws_s3_bucket_versioning" "s3_ran_bucket_versioning" {
  bucket = aws_s3_bucket.s3_ran_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_s3_bucket.bucket** | nautilus-s3-5581 | Name of the S3 bucket |
| **aws_s3_bucket.tags.Name** | nautilus-s3-5581 | Tag for bucket identification |
| **aws_s3_bucket_versioning.bucket** | (Dynamic) | ID of the S3 bucket |
| **aws_s3_bucket_versioning.versioning_configuration.status** | Enabled | Enables versioning for the bucket |

### Resource Properties
- **Region:** S3 is global, but the provider requires a region (`us-east-1` used for consistency).
- **Versioning:** Enables multiple versions of objects to be stored for recovery.
- **Access Control:** Bucket is private by default (no public access).

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_s3_bucket.s3_ran_bucket
aws_s3_bucket_versioning.s3_ran_bucket_versioning
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed versioning configuration
terraform state show aws_s3_bucket_versioning.s3_ran_bucket_versioning
```

**Expected Output:**
```
# aws_s3_bucket_versioning.s3_ran_bucket_versioning:
resource "aws_s3_bucket_versioning" "s3_ran_bucket_versioning" {
    bucket  = "nautilus-s3-5581"
    id      = "nautilus-s3-5581"
    versioning_configuration {
        status = "Enabled"
    }
}
```

### Step 3: Verify in AWS Console (Optional)

```bash
# Check versioning status
aws s3api get-bucket-versioning --bucket nautilus-s3-5581
```

**Expected JSON Output:**
```json
{
    "Status": "Enabled"
}
```

---

## 🧪 Testing

### Verify Versioning Functionality

1. Upload an object to the bucket:
```bash
echo "Test content v1" > test.txt
aws s3 cp test.txt s3://nautilus-s3-5581/test.txt
```

2. Modify and re-upload the object:
```bash
echo "Test content v2" > test.txt
aws s3 cp test.txt s3://nautilus-s3-5581/test.txt
```

3. List object versions:
```bash
aws s3api list-object-versions --bucket nautilus-s3-5581 --prefix test.txt
```

**Expected Output (partial):**
```json
{
    "Versions": [
        {
            "Key": "test.txt",
            "VersionId": "v2_id",
            "IsLatest": true,
            "LastModified": "2025-10-18T17:30:00Z"
        },
        {
            "Key": "test.txt",
            "VersionId": "v1_id",
            "IsLatest": false,
            "LastModified": "2025-10-18T17:29:00Z"
        }
    ]
}
```

**Note:** This confirms versioning is enabled, as multiple versions of `test.txt` are stored.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage S3 bucket
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-5581"

  tags = {
    Name = "nautilus-s3-5581"
  }
}

# Enable versioning for the S3 bucket
resource "aws_s3_bucket_versioning" "s3_ran_bucket_versioning" {
  bucket = aws_s3_bucket.s3_ran_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
# Import existing bucket (if it exists)
terraform import aws_s3_bucket.s3_ran_bucket nautilus-s3-5581
terraform init
terraform apply -auto-approve
terraform plan
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_s3_bucket_versioning.s3_ran_bucket_versioning

# Verify via AWS CLI
aws s3api get-bucket-versioning --bucket nautilus-s3-5581
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Bucket Already Exists**

- **Symptoms:** Error: "BucketAlreadyExists: The requested bucket name is not available"
- **Solution:** Import the existing bucket
```bash
terraform import aws_s3_bucket.s3_ran_bucket nautilus-s3-5581
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: s3:PutBucketVersioning"
- **Solution:** Ensure AWS credentials have S3 permissions
```bash
# Required permissions:
# - s3:CreateBucket
# - s3:PutBucketVersioning
# - s3:GetBucketVersioning
```

**Issue 3: Terraform Plan Shows Changes**

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

- **🔐 Security:** Ensured the bucket remains private by default.
- **📊 Resource Naming:** Used clear names (`nautilus-s3-5581`) via tags and attributes.
- **🏷️ Minimal Configuration:** Focused on enabling versioning, preserving existing settings.
- **🔄 Resource Import:** Imported existing bucket to manage with Terraform.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### S3 Data Protection Framework
- **Versioning:** Protects against accidental deletions and overwrites.
- **Lifecycle Rules:** Implement rules to manage old versions and reduce storage costs.
- **Monitoring:** Enable AWS CloudTrail to log S3 actions.
- **Access Control:** Use bucket policies or IAM roles for fine-grained access.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-5581"

  tags = {
    Name        = "nautilus-s3-5581"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "data-protection"
  }
}

resource "aws_s3_bucket_versioning" "s3_ran_bucket_versioning" {
  bucket = aws_s3_bucket.s3_ran_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

output "bucket_arn" {
  description = "ARN of the S3 bucket"
  value       = aws_s3_bucket.s3_ran_bucket.arn
}
```

### Security Strategy
- **Access Control:** Implement bucket policies to restrict access.
- **Encryption:** Enable server-side encryption for data at rest.
- **Monitoring:** Set up CloudTrail to audit S3 actions.
- **Cost Optimization:** Use lifecycle policies to transition old versions to cheaper storage classes (e.g., Glacier).

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **S3 Fundamentals:** Understanding S3 buckets and versioning.
- **Versioning Management:** Enabling versioning with Terraform.
- **Resource Import:** Importing existing S3 buckets into Terraform state.
- **Dependency Management:** Ensuring proper resource dependencies in Terraform.

**Terraform Features Used:**
- `aws_s3_bucket`: Managing the S3 bucket.
- `aws_s3_bucket_versioning`: Enabling versioning for the bucket.
- AWS provider integration for S3 services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Versioning Enabled**: Enabled versioning for the `nautilus-s3-5581` S3 bucket.
- **File Structure**: Updated single `main.tf` file as specified.
- **Verification**: Confirmed versioning via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Data Protection Foundation:** The S3 bucket `nautilus-s3-5581` now has versioning enabled, ensuring data recovery capabilities for the Nautilus DevOps team’s application.

### 🔮 Resources Ready for:
- **Data Recovery**: Recover objects from accidental deletions or overwrites.
- **Monitoring**: Audit S3 actions with CloudTrail.
- **Security**: Enhance with bucket policies and encryption.
- **Cost Management**: Implement lifecycle rules for old versions.