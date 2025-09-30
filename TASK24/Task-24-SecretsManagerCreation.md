# 🌟 Task 24 - Create AWS Secrets Manager Secret Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** needs to store sensitive data securely using AWS Secrets Manager to manage application credentials. They require a secret with specific key-value pairs to be created and managed programmatically.

**Requirements:**
- Create an AWS Secrets Manager secret named **`xfusion-secret`** using Terraform.
- The secret value should contain a key-value pair: `username: admin`, `password: Namin123`.
- The Terraform working directory is **`/home/bob/terraform`**.
- Create the **`main.tf`** file (do not create a different `.tf` file).

👉 **Your task:** Use Terraform to create a Secrets Manager secret named `xfusion-secret` with the specified key-value pair to enable secure storage of credentials for the Nautilus DevOps team.

💡 **Note:** AWS Secrets Manager is a service for securely storing, managing, and retrieving sensitive data like credentials. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable via Terraform and AWS CLI. The current date and time is September 30, 2025, 10:29 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (Secrets Manager, region-specific)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- Secrets Manager secret for storing sensitive data
- Secret version with key-value pair (`username: admin`, `password: Namin123`)
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Secrets Manager Secret Resource:** Creates a secret named `xfusion-secret`.
- **Secret Version Resource:** Stores the key-value pair (`username: admin`, `password: Namin123`) as a JSON string.
- **Security Framework:** Prepares for secure credential storage and retrieval.

### 🎯 Implementation Strategy
1. Create a Terraform configuration in `main.tf` to define the `aws_secretsmanager_secret` and `aws_secretsmanager_secret_version` resources.
2. Configure the secret with the name `xfusion-secret`.
3. Set the secret value as a JSON-encoded string with the specified key-value pair.
4. Deploy the secret using Terraform in the specified directory.
5. Verify the secret creation using Terraform state and AWS CLI.

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

# Create Secrets Manager secret for storing sensitive data
resource "aws_secretsmanager_secret" "xfusion" {
  name = "xfusion-secret"
}

# Create secret version with key-value pair
resource "aws_secretsmanager_secret_version" "xfusion_value" {
  secret_id     = aws_secretsmanager_secret.xfusion.id
  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (adjust if a different region is required).
- `aws_secretsmanager_secret`: Defines a Secrets Manager secret resource.
- `name = "xfusion-secret"`: Sets the secret name as required.
- `aws_secretsmanager_secret_version`: Defines the secret version with the key-value pair.
- `secret_id = aws_secretsmanager_secret.xfusion.id`: Links the version to the secret.
- `secret_string = jsonencode({...})`: Encodes the key-value pair (`username: admin`, `password: Namin123`) as a JSON string.

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

  # aws_secretsmanager_secret.xfusion will be created
  + resource "aws_secretsmanager_secret" "xfusion" {
      + arn       = (known after apply)
      + id        = (known after apply)
      + name      = "xfusion-secret"
      + tags_all  = (known after apply)
    }

  # aws_secretsmanager_secret_version.xfusion_value will be created
  + resource "aws_secretsmanager_secret_version" "xfusion_value" {
      + arn            = (known after apply)
      + id             = (known after apply)
      + secret_id      = (known after apply)
      + secret_string  = jsonencode(
            {
              password = "Namin123"
              username = "admin"
            }
        )
      + version_id     = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

### Step 6: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_secretsmanager_secret.xfusion: Refreshing state... [id=arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-...]
aws_secretsmanager_secret_version.xfusion_value: Refreshing state... [id=arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-...]

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

# Create Secrets Manager secret for storing sensitive data
resource "aws_secretsmanager_secret" "xfusion" {
  name = "xfusion-secret"
}

# Create secret version with key-value pair
resource "aws_secretsmanager_secret_version" "xfusion_value" {
  secret_id     = aws_secretsmanager_secret.xfusion.id
  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
}
```

### Secrets Manager Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **Secret: name** | xfusion-secret | Name of the Secrets Manager secret |
| **Secret Version: secret_id** | (Dynamic) | ID of the associated secret |
| **Secret Version: secret_string** | `{"username":"admin","password":"Namin123"}` | JSON-encoded key-value pair |
| **Recovery Window** | 30 days (default) | Default deletion recovery period |

### Secrets Manager Default Properties
- **Region:** `us-east-1` (specified in provider, adjust if needed).
- **Encryption:** Automatically encrypted using AWS-managed KMS key.
- **Access Policy:** Controlled by IAM permissions (open to creator by default).
- **Recovery Window:** 30-day recovery period for deleted secrets (can be set to 0 for immediate deletion).

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_secretsmanager_secret.xfusion
aws_secretsmanager_secret_version.xfusion_value
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed secret information
terraform state show aws_secretsmanager_secret.xfusion
```

**Expected Output:**
```
# aws_secretsmanager_secret.xfusion:
resource "aws_secretsmanager_secret" "xfusion" {
    arn       = "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-..."
    id        = "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-..."
    name      = "xfusion-secret"
    tags_all  = {}
}
```

```bash
# Show detailed secret version information
terraform state show aws_secretsmanager_secret_version.xfusion_value
```

**Expected Output:**
```
# aws_secretsmanager_secret_version.xfusion_value:
resource "aws_secretsmanager_secret_version" "xfusion_value" {
    arn           = "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-..."
    id            = "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-..."
    secret_id     = "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-..."
    secret_string = jsonencode(
        {
            password = "Namin123"
            username = "admin"
        }
    )
    version_id    = "..."
}
```

### Step 3: Verify Resources in AWS Console (Optional)

```bash
# List Secrets Manager secrets using AWS CLI
aws secretsmanager list-secrets --filter Key=name,Values=xfusion-secret
```

**Expected JSON Output (partial):**
```json
{
    "SecretList": [
        {
            "ARN": "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-...",
            "Name": "xfusion-secret",
            "CreatedDate": "2025-09-30T22:29:00Z"
        }
    ]
}
```

```bash
# Retrieve the secret value (requires appropriate permissions)
aws secretsmanager get-secret-value --secret-id xfusion-secret
```

**Expected JSON Output (partial):**
```json
{
    "ARN": "arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-...",
    "Name": "xfusion-secret",
    "VersionId": "...",
    "SecretString": "{\"username\":\"admin\",\"password\":\"Namin123\"}",
    "CreatedDate": "2025-09-30T22:29:00Z"
}
```

---

## 🧪 Testing

### Verify Resource Creation

```bash
# Show complete secret details from Terraform state
terraform state show aws_secretsmanager_secret.xfusion
terraform state show aws_secretsmanager_secret_version.xfusion_value
```

### Test Secret Retrieval (Optional)

```bash
# Retrieve the secret value using AWS CLI
aws secretsmanager get-secret-value --secret-id xfusion-secret --query SecretString --output text
```

**Expected Output:**
```json
{"username":"admin","password":"Namin123"}
```

```bash
# Parse the secret value using jq
aws secretsmanager get-secret-value --secret-id xfusion-secret --query SecretString --output text | jq .
```

**Expected Output:**
```json
{
  "username": "admin",
  "password": "Namin123"
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

# Create Secrets Manager secret for storing sensitive data
resource "aws_secretsmanager_secret" "xfusion" {
  name = "xfusion-secret"
}

# Create secret version with key-value pair
resource "aws_secretsmanager_secret_version" "xfusion_value" {
  secret_id     = aws_secretsmanager_secret.xfusion.id
  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
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
terraform state show aws_secretsmanager_secret.xfusion
terraform state show aws_secretsmanager_secret_version.xfusion_value

# Verify via AWS CLI
aws secretsmanager list-secrets --filter Key=name,Values=xfusion-secret
aws secretsmanager get-secret-value --secret-id xfusion-secret
```

### Optional: Enhanced Configuration

```hcl
# Enhanced Secrets Manager secret with production-ready settings
provider "aws" {
  region = "us-east-1"
}

resource "aws_secretsmanager_secret" "xfusion" {
  name                    = "xfusion-secret"
  description             = "Secret for Nautilus application credentials"
  recovery_window_in_days = 7

  tags = {
    Name        = "xfusion-secret"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "credential-storage"
  }
}

resource "aws_secretsmanager_secret_version" "xfusion_value" {
  secret_id     = aws_secretsmanager_secret.xfusion.id
  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
}

output "secret_arn" {
  description = "ARN of the created Secrets Manager secret"
  value       = aws_secretsmanager_secret.xfusion.arn
}

output "secret_name" {
  description = "Name of the created Secrets Manager secret"
  value       = aws_secretsmanager_secret.xfusion.name
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Secret Already Exists**

- **Symptoms:** Error: "ResourceAlreadyExistsException: The operation failed because the secret xfusion-secret already exists"
- **Solution:** Import the existing secret or use a different secret name
```bash
# Import existing secret into Terraform state
terraform import aws_secretsmanager_secret.xfusion arn:aws:secretsmanager:us-east-1:000000000000:secret:xfusion-secret-...
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDeniedException: User is not authorized to perform: secretsmanager:CreateSecret"
- **Solution:** Ensure AWS credentials have Secrets Manager permissions
```bash
# Required permissions:
# - secretsmanager:CreateSecret
# - secretsmanager:PutSecretValue
# - secretsmanager:ListSecrets
# - secretsmanager:GetSecretValue
# - secretsmanager:DescribeSecret
```

**Issue 3: Invalid Secret String**

- **Symptoms:** Error: "InvalidParameterException: Invalid secret string"
- **Solution:** Ensure the `secret_string` is valid JSON
```bash
# Validate JSON syntax
echo '{"username":"admin","password":"Namin123"}' | jq .
```

**Issue 4: Terraform Plan Shows Changes**

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

- **🔐 Security:** Stored sensitive data securely in Secrets Manager with automatic encryption.
- **📊 Resource Naming:** Clear secret name (`xfusion-secret`) matching requirements.
- **🏷️ Minimal Configuration:** Focused setup with minimal attributes for simplicity.
- **🔄 Dependency Management:** Secret version correctly references the secret resource.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### Secrets Manager Security Framework
- **Access Control:** Restrict secret access with IAM policies.
- **Encryption:** Use a custom KMS key for encryption (default is AWS-managed).
- **Rotation:** Enable secret rotation for dynamic credentials (e.g., using AWS Lambda).
- **Monitoring:** Enable CloudWatch metrics for secret access tracking.
- **Tagging:** Add tags for cost allocation and resource management.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_secretsmanager_secret" "xfusion" {
  name                    = "xfusion-secret"
  description             = "Secret for Nautilus application credentials"
  recovery_window_in_days = 7

  tags = {
    Name        = "xfusion-secret"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "credential-storage"
  }
}

resource "aws_secretsmanager_secret_version" "xfusion_value" {
  secret_id     = aws_secretsmanager_secret.xfusion.id
  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
}

resource "aws_secretsmanager_secret_policy" "xfusion_policy" {
  secret_arn = aws_secretsmanager_secret.xfusion.arn

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect    = "Allow"
        Principal = { AWS = "arn:aws:iam::000000000000:role/nautilus-devops-role" }
        Action    = ["secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret"]
        Resource  = aws_secretsmanager_secret.xfusion.arn
      }
    ]
  })
}
```

### Security Strategy
- **Access Policies:** Restrict access to specific IAM roles/users.
- **Rotation:** Implement rotation for dynamic credentials (e.g., database passwords).
- **Monitoring:** Set up CloudWatch alarms for unauthorized access attempts.
- **Cost Optimization:** Monitor secret usage to manage costs.
- **Backup:** Use the recovery window to prevent accidental deletion.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Secrets Manager Fundamentals:** Understanding secret creation and management.
- **Secure Storage:** Storing sensitive data with key-value pairs.
- **Terraform Integration:** Managing AWS Secrets Manager with Terraform.
- **Dependency Management:** Ensuring proper resource dependencies in Terraform.

**Terraform Features Used:**
- `aws_secretsmanager_secret`: Secret resource creation.
- `aws_secretsmanager_secret_version`: Secret version creation with JSON-encoded data.
- AWS provider integration for Secrets Manager services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Secret Name**: Created `xfusion-secret` as required.
- **Secret Value**: Stored `username: admin`, `password: Namin123` as a JSON-encoded string.
- **File Structure**: Used single `main.tf` file as specified.
- **Verification**: Confirmed secret creation via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Security Foundation:** The Secrets Manager secret `xfusion-secret` is created with the specified credentials, ready for the Nautilus DevOps team to securely store and retrieve application credentials.

### 🔮 Resources Ready for:
- **Credential Storage**: Store sensitive data securely.
- **Retrieval**: Access credentials via AWS SDK, CLI, or applications.
- **Security**: Implement access policies and rotation.
- **Monitoring**: Track secret access with CloudWatch.
- **Integration**: Connect with applications for credential management.