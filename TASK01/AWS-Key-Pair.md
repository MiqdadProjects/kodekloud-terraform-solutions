```markdown
# 🌟 Task 1 - AWS Key Pair Creation with Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- Name of the key pair should be **`xfusion-kp`**
- Key pair type must be **`rsa`**
- The private key file should be saved under **`/home/bob/xfusion-kp.pem`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create a key pair using Terraform with the specified requirements for AWS infrastructure migration preparation.

💡 **Note:** Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- AWS Key Pair for EC2 instance access
- RSA private key generation
- Local file storage for private key

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **TLS Private Key Resource:** Generates RSA private key locally using Terraform
- **AWS Key Pair Resource:** Creates the key pair in AWS using the generated public key
- **Local File Resource:** Saves the private key to the specified local path with proper permissions

### 🎯 Implementation Strategy
1. Generate RSA private key locally using `tls_private_key` resource
2. Create AWS key pair using the public key portion
3. Save private key file locally with secure permissions (0600)

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

# Generate RSA private key locally
resource "tls_private_key" "xfusion_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# Create AWS key pair using the public key
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_key.public_key_openssh
}

# Save private key locally
resource "local_file" "private_key_pem" {
  content         = tls_private_key.xfusion_key.private_key_pem
  filename        = "/home/bob/xfusion-kp.pem"
  file_permission = "0600"
}
```

**Configuration Breakdown:**
- `tls_private_key` - Generates a 4096-bit RSA private key
- `aws_key_pair` - Creates AWS key pair with name "xfusion-kp"
- `local_file` - Saves private key with secure 0600 permissions

---

### Step 3: Initialize Terraform
```bash
terraform init
```

**Purpose:** Initialize the Terraform working directory and download required providers.

**Expected Output:**
```
Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/tls...
- Finding latest version of hashicorp/aws...
- Finding latest version of hashicorp/local...
- Installing hashicorp/tls v4.0.4...
- Installing hashicorp/aws v5.20.0...
- Installing hashicorp/local v2.4.0...

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

  # aws_key_pair.xfusion_kp will be created
  + resource "aws_key_pair" "xfusion_kp" {
      + key_name     = "xfusion-kp"
      + key_type     = "rsa"
      + public_key   = (known after apply)
    }

  # local_file.private_key_pem will be created
  + resource "local_file" "private_key_pem" {
      + content         = (sensitive value)
      + filename        = "/home/bob/xfusion-kp.pem"
      + file_permission = "0600"
    }

  # tls_private_key.xfusion_key will be created
  + resource "tls_private_key" "xfusion_key" {
      + algorithm   = "RSA"
      + rsa_bits    = 4096
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Generate RSA private key locally
resource "tls_private_key" "xfusion_key" {
  algorithm = "RSA"      # Specifies RSA algorithm as required
  rsa_bits  = 4096       # Uses 4096 bits for enhanced security
}

# Create AWS key pair using the public key
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"                                    # Required key pair name
  public_key = tls_private_key.xfusion_key.public_key_openssh  # References generated public key
}

# Save private key locally with secure permissions
resource "local_file" "private_key_pem" {
  content         = tls_private_key.xfusion_key.private_key_pem  # Private key in PEM format
  filename        = "/home/bob/xfusion-kp.pem"                   # Required file path
  file_permission = "0600"                                       # Secure permissions (read/write owner only)
}
```

### Resource Dependencies
- `aws_key_pair` depends on `tls_private_key` for the public key
- `local_file` depends on `tls_private_key` for the private key content
- Terraform automatically handles the dependency order

---

## ✅ Verification Steps

### Step 1: Verify Key Pair Creation in AWS
```bash
# List Terraform state to confirm resources
terraform state list
```

**Expected Output:**
```
aws_key_pair.xfusion_kp
local_file.private_key_pem
tls_private_key.xfusion_key
```

### Step 2: Check Private Key File
```bash
# Verify private key file was created with correct permissions
ls -l /home/bob/xfusion-kp.pem
```

**Expected Output:**
```
-rw------- 1 bob bob 3247 [date] /home/bob/xfusion-kp.pem
```

### Step 3: Validate Key File Format
```bash
# Check if the private key file is valid
head -n 1 /home/bob/xfusion-kp.pem
```

**Expected Output:**
```
-----BEGIN RSA PRIVATE KEY-----
```

---

## 🧪 Testing

### Verify AWS Key Pair Exists
```bash
# Show key pair details from Terraform state
terraform show | grep -A 5 "aws_key_pair"
```

### Test Private Key Integrity
```bash
# Verify private key format and structure
openssl rsa -in /home/bob/xfusion-kp.pem -check -noout
```

**Expected Result:**
```
RSA key ok
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Generate RSA private key locally
resource "tls_private_key" "xfusion_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# Create AWS key pair using the public key
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_key.public_key_openssh
}

# Save private key locally
resource "local_file" "private_key_pem" {
  content         = tls_private_key.xfusion_key.private_key_pem
  filename        = "/home/bob/xfusion-kp.pem"
  file_permission = "0600"
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply -auto-approve
ls -l /home/bob/xfusion-kp.pem
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Permission denied when creating private key file**
- **Symptoms:** Error creating `/home/bob/xfusion-kp.pem`
- **Solution:** Ensure the directory exists and has proper write permissions
```bash
# Ensure directory exists
mkdir -p /home/bob
# Check permissions
ls -ld /home/bob
```

**Issue 2: AWS credentials not configured**
- **Symptoms:** Error: "No valid credential sources found"
- **Solution:** Configure AWS credentials
```bash
# Configure AWS CLI
aws configure
# Or set environment variables
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
```

**Issue 3: Key pair already exists in AWS**
- **Symptoms:** Error: "InvalidKeyPair.Duplicate"
- **Solution:** Import existing key pair or use different name
```bash
# Import existing key pair (if you have the public key)
terraform import aws_key_pair.xfusion_kp xfusion-kp
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Private key stored with restrictive 0600 permissions
- **📊 Resource Management:** Using resource dependencies for proper creation order
- **🏷️ Naming Conventions:** Clear, descriptive resource names matching requirements
- **🔄 State Management:** All resources managed in Terraform state for consistency

---

## 🚀 Production Considerations

- **Scalability:** Key pairs can be referenced across multiple EC2 instances
- **Security:** Consider using AWS Systems Manager Parameter Store for sensitive data
- **Backup/Recovery:** Store private keys securely in encrypted storage
- **Access Control:** Implement IAM policies to control key pair management

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **TLS Private Key Generation:** Creating RSA keys using Terraform's TLS provider
- **AWS Key Pair Management:** Registering public keys with AWS for EC2 access
- **Local File Management:** Securely storing private keys with appropriate permissions

**Terraform Features Used:**
- `tls_private_key` - Cryptographic key generation
- `aws_key_pair` - AWS resource creation
- `local_file` - Local filesystem management
- Resource dependencies and references

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Key Pair Name** - Created "xfusion-kp" as required
- **Key Type** - Used RSA algorithm with 4096-bit key size
- **Private Key Storage** - Saved to `/home/bob/xfusion-kp.pem` with secure permissions
- **File Structure** - Used single `main.tf` file as specified

**Final Status:** 🎉 **Task completed successfully with all requirements met**

The AWS key pair is now ready for use in EC2 instance deployments as part of the Nautilus DevOps team's incremental cloud migration strategy.
```