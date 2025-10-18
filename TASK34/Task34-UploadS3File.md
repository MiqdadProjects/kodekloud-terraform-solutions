# 🌟 Task 34 - Upload File to AWS S3 Bucket Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is engaged in data migrations, transferring data from on-premise storage systems to AWS S3 buckets. They need to copy a specific file to an existing S3 bucket as part of this process.

**Requirements:**
- Copy the file `/tmp/devops.txt` to the S3 bucket named **`devops-cp-30088`** using Terraform.
- The S3 bucket `devops-cp-30088` already exists.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update the **`main.tf`** file (do not create a separate `.tf` file).
- Ensure the file upload is verifiable via Terraform and AWS CLI.

💡 **Note:** The task involves uploading `/tmp/devops.txt` to the existing `devops-cp-30088` bucket. The configuration must account for the bucket's pre-existence, and the action must be verifiable. The current date and time is October 18, 2025, 06:01 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (S3, region-agnostic)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- S3 bucket named `devops-cp-30088` (pre-existing)
- S3 object `/tmp/devops.txt` to be uploaded as `devops.txt`
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **S3 Bucket Resource:** References the existing `devops-cp-30088` bucket.
- **S3 Object Resource:** Manages the upload of `/tmp/devops.txt` to the bucket.
- **Security Framework:** Ensures the bucket remains private with appropriate access controls.

### 🎯 Implementation Strategy
1. Verify the existence of the `devops-cp-30088` S3 bucket and the `/tmp/devops.txt` file.
2. Update the `main.tf` file to include `aws_s3_bucket` and `aws_s3_object` resources.
3. Import the existing bucket into the Terraform state to manage it.
4. Deploy the configuration to upload the file.
5. Verify the file upload using Terraform state and AWS CLI.

---

## 🚀 Implementation Steps

### Step 1: Verify Existing Resources

Confirm the `devops-cp-30088` bucket exists and the `/tmp/devops.txt` file is available.

```bash
# Check the S3 bucket
aws s3api get-bucket-location --bucket devops-cp-30088
```

**Example Output:**
```json
{
    "LocationConstraint": "us-east-1"
}
```

```bash
# Check the local file
ls -l /tmp/devops.txt
```

**Example Output:**
```
-rw-r--r-- 1 bob bob 1234 Oct 18 17:00 /tmp/devops.txt
```

**Note:** Ensure the file `/tmp/devops.txt` exists and AWS CLI is configured with credentials that have S3 permissions.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Update Main Terraform Configuration

Update the `main.tf` file to manage the existing S3 bucket and upload the file:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"  # S3 is global, but provider requires a region
}

# Manage existing S3 bucket
resource "aws_s3_bucket" "my_bucket" {
  bucket = "devops-cp-30088"

  tags = {
    Name = "devops-cp-30088"
  }
}

# Upload file to S3 bucket
resource "aws_s3_object" "upload_file" {
  bucket = aws_s3_bucket.my_bucket.bucket
  key    = "devops.txt"
  source = "/tmp/devops.txt"
  etag   = filemd5("/tmp/devops.txt")
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (required for Terraform, though S3 is global).
- `aws_s3_bucket.my_bucket`: References the existing `devops-cp-30088` bucket with a private ACL (default in Terraform AWS provider v4+).
- `aws_s3_object.upload_file`: Uploads `/tmp/devops.txt` to the bucket as `devops.txt`, with `etag` to detect file changes.
- `tags`: Applies the name `devops-cp-30088` for identification.

**Note:** The `acl` attribute is deprecated in newer Terraform AWS provider versions. The bucket is private by default, aligning with the provided solution's `acl = "private"`.

---

### Step 4: Import Existing Bucket

Since the `devops-cp-30088` bucket already exists, import it into the Terraform state.

```bash
# Import the S3 bucket
terraform import aws_s3_bucket.my_bucket devops-cp-30088
```

**Note:** If the bucket does not exist, Terraform will attempt to create it, but the task specifies it already exists, so importing is necessary.

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

  # aws_s3_bucket.my_bucket will be created
  + resource "aws_s3_bucket" "my_bucket" {
      + bucket        = "devops-cp-30088"
      + id            = (known after apply)
      + tags          = {
          + "Name" = "devops-cp-30088"
        }
    }

  # aws_s3_object.upload_file will be created
  + resource "aws_s3_object" "upload_file" {
      + bucket       = "devops-cp-30088"
      + etag         = (known after apply)
      + id           = (known after apply)
      + key          = "devops.txt"
      + source       = "/tmp/devops.txt"
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

**Note:** If the bucket was imported, the output will show only the `aws_s3_object.upload_file` resource being created.

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_s3_bucket.my_bucket: Refreshing state... [id=devops-cp-30088]
aws_s3_object.upload_file: Refreshing state... [id=devops.txt]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

### Step 9: Verify File Upload

Confirm the file `devops.txt` is in the S3 bucket using AWS CLI.

```bash
# List objects in the bucket
aws s3 ls s3://devops-cp-30088/
```

**Expected Output:**
```
2025-10-18 17:00:00       1234 devops.txt
```

```bash
# Download and verify the file
aws s3 cp s3://devops-cp-30088/devops.txt /tmp/devops-downloaded.txt
diff /tmp/devops.txt /tmp/devops-downloaded.txt
```

**Expected Output (diff):**
```
# No output indicates files are identical
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing S3 bucket
resource "aws_s3_bucket" "my_bucket" {
  bucket = "devops-cp-30088"

  tags = {
    Name = "devops-cp-30088"
  }
}

# Upload file to S3 bucket
resource "aws_s3_object" "upload_file" {
  bucket = aws_s3_bucket.my_bucket.bucket
  key    = "devops.txt"
  source = "/tmp/devops.txt"
  etag   = filemd5("/tmp/devops.txt")
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_s3_bucket.my_bucket.bucket** | devops-cp-30088 | Name of the S3 bucket |
| **aws_s3_bucket.my_bucket.tags.Name** | devops-cp-30088 | Tag for bucket identification |
| **aws_s3_object.upload_file.bucket** | devops-cp-30088 | Target bucket for the file |
| **aws_s3_object.upload_file.key** | devops.txt | Object name in S3 |
| **aws_s3_object.upload_file.source** | /tmp/devops.txt | Local file path |
| **aws_s3_object.upload_file.etag** | filemd5("/tmp/devops.txt") | MD5 hash to detect file changes |

### Resource Properties
- **Region:** S3 is global, but the provider requires a region (`us-east-1` used for consistency).
- **File Upload:** Uploads `/tmp/devops.txt` as `devops.txt` in the bucket.
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
aws_s3_bucket.my_bucket
aws_s3_object.upload_file
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show details of the uploaded file
terraform state show aws_s3_object.upload_file
```

**Expected Output:**
```
# aws_s3_object.upload_file:
resource "aws_s3_object" "upload_file" {
    bucket       = "devops-cp-30088"
    etag         = "<md5-hash>"
    id           = "devops.txt"
    key          = "devops.txt"
    source       = "/tmp/devops.txt"
}
```

### Step 3: Verify in AWS Console (Optional)

```bash
# Check objects in the bucket
aws s3 ls s3://devops-cp-30088/
```

**Expected Output:**
```
2025-10-18 17:00:00       1234 devops.txt
```

---

## 🧪 Testing

### Verify File Upload Functionality

1. List objects in the bucket:
```bash
aws s3 ls s3://devops-cp-30088/
```

**Expected Output:**
```
2025-10-18 17:00:00       1234 devops.txt
```

2. Download and compare the file:
```bash
aws s3 cp s3://devops-cp-30088/devops.txt /tmp/devops-downloaded.txt
diff /tmp/devops.txt /tmp/devops-downloaded.txt
```

**Expected Output:** No differences, confirming the file was uploaded correctly.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing S3 bucket
resource "aws_s3_bucket" "my_bucket" {
  bucket = "devops-cp-30088"

  tags = {
    Name = "devops-cp-30088"
  }
}

# Upload file to S3 bucket
resource "aws_s3_object" "upload_file" {
  bucket = aws_s3_bucket.my_bucket.bucket
  key    = "devops.txt"
  source = "/tmp/devops.txt"
  etag   = filemd5("/tmp/devops.txt")
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
# Import existing bucket
terraform import aws_s3_bucket.my_bucket devops-cp-30088
terraform init
terraform apply -auto-approve
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_s3_object.upload_file

# Verify via AWS CLI
aws s3 ls s3://devops-cp-30088/
aws s3 cp s3://devops-cp-30088/devops.txt /tmp/devops-downloaded.txt
diff /tmp/devops.txt /tmp/devops-downloaded.txt
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Bucket Already Exists**

- **Symptoms:** Error: "BucketAlreadyExists: The requested bucket name is not available"
- **Solution:** Import the existing bucket
```bash
terraform import aws_s3_bucket.my_bucket devops-cp-30088
```

**Issue 2: File Not Found**

- **Symptoms:** Error: "No such file or directory: /tmp/devops.txt"
- **Solution:** Ensure the file exists
```bash
ls -l /tmp/devops.txt
# Create a sample file if needed
echo "Sample data" > /tmp/devops.txt
```

**Issue 3: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: Access Denied"
- **Solution:** Ensure AWS credentials have S3 permissions
```bash
# Required permissions:
# - s3:PutObject
# - s3:GetBucketLocation
# - s3:ListBucket
```

**Issue 4: ETag Mismatch**

- **Symptoms:** Terraform reapplies due to file changes
- **Solution:** Ensure the file is not modified during Terraform execution
```bash
terraform apply -auto-approve
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Ensured the bucket remains private by default.
- **📊 Resource Naming:** Used clear names (`devops-cp-30088`, `devops.txt`) for resources.
- **🏷️ Minimal Configuration:** Focused on uploading the file while managing the existing bucket.
- **🔄 Resource Import:** Imported the existing bucket to manage with Terraform.
- **📍 Verifiability:** Ensured the upload is verifiable via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### S3 Data Migration Framework
- **File Integrity:** Use `etag` to verify file consistency during uploads.
- **Monitoring:** Enable AWS CloudTrail to log S3 actions for auditing.
- **Access Control:** Use bucket policies or IAM roles for fine-grained access.
- **Versioning:** Consider enabling versioning on the bucket for data recovery.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "devops-cp-30088"

  tags = {
    Name        = "devops-cp-30088"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "data-migration"
  }
}

resource "aws_s3_object" "upload_file" {
  bucket = aws_s3_bucket.my_bucket.bucket
  key    = "devops.txt"
  source = "/tmp/devops.txt"
  etag   = filemd5("/tmp/devops.txt")

  tags = {
    Name = "devops.txt"
  }
}

output "object_key" {
  description = "Key of the uploaded S3 object"
  value       = aws_s3_object.upload_file.key
}
```

### Security Strategy
- **Access Control:** Implement bucket policies to restrict access.
- **Encryption:** Enable server-side encryption for data at rest.
- **Monitoring:** Set up CloudTrail to audit S3 actions.
- **Validation:** Verify file integrity post-upload using checksums.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **S3 Data Migration:** Uploading files to S3 using Terraform.
- **Resource Import:** Managing existing S3 buckets with Terraform.
- **File Management:** Ensuring file integrity with `etag` in S3 uploads.
- **Dependency Management:** Linking S3 bucket and object resources in Terraform.

**Terraform Features Used:**
- `aws_s3_bucket`: Managing the S3 bucket.
- `aws_s3_object`: Uploading files to S3.
- AWS provider integration for S3 services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **File Uploaded**: Copied `/tmp/devops.txt` to the `devops-cp-30088` bucket as `devops.txt`.
- **Bucket Managed**: Imported and managed the existing `devops-cp-30088` bucket.
- **Verification**: Confirmed the file upload via Terraform state and AWS CLI.
- **File Structure**: Updated single `main.tf` file as specified.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Migration Foundation:** The file `/tmp/devops.txt` has been successfully uploaded to the `devops-cp-30088` S3 bucket, supporting the Nautilus DevOps team’s data migration efforts.

### 🔮 Resources Ready for:
- **Data Access**: Access the uploaded file in S3 for further processing.
- **Monitoring**: Audit S3 actions with CloudTrail.
- **Security**: Enhance with bucket policies and encryption.
- **Scalability**: Extend to upload multiple files using loops or modules.