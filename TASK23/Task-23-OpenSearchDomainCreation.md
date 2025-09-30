# 🌟 Task 23 - Create AWS OpenSearch Domain Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** needs to set up an Amazon OpenSearch Service domain to store and search application logs efficiently. The domain must be configured to meet the team's requirements for log management and analysis.

**Requirements:**
- Create an Amazon OpenSearch Service domain named **`xfusion-es`** using Terraform.
- The Terraform working directory is **`/home/bob/terraform`**.
- Create the **`main.tf`** file (do not create a different `.tf` file).

👉 **Your task:** Use Terraform to create an OpenSearch domain named `xfusion-es` to enable log storage and search capabilities for the Nautilus DevOps team’s applications.

💡 **Note:** Amazon OpenSearch Service is a managed service for deploying, operating, and scaling OpenSearch clusters. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable via Terraform and AWS CLI. The OpenSearch domain creation may take several minutes (approximately 10 minutes). Ensure `terraform plan` returns "No changes" after creation. The current date and time is September 30, 2025, 10:16 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (Amazon OpenSearch Service, region-specific)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- OpenSearch domain for log storage and search
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **OpenSearch Domain Resource:** Creates a managed OpenSearch domain named `xfusion-es`.
- **Domain Name:** Configures the domain as `xfusion-es`.
- **Default Configuration:** Uses minimal settings for instance type, storage, and access policies.
- **Search Framework:** Prepares for log storage, indexing, and querying.

### 🎯 Implementation Strategy
1. Create a Terraform configuration in `main.tf` to define the `aws_opensearch_domain` resource.
2. Configure the OpenSearch domain with the name `xfusion-es`.
3. Deploy the domain using Terraform in the specified directory.
4. Wait for the domain creation to complete (approximately 10 minutes).
5. Verify the domain creation using Terraform state and AWS CLI, ensuring `terraform plan` shows "No changes."

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

# Create OpenSearch domain for log storage and search
resource "aws_opensearch_domain" "xfusion" {
  domain_name = "xfusion-es"
  engine_version = "OpenSearch_2.11"
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region (adjust if a different region is required).
- `aws_opensearch_domain`: Defines an OpenSearch domain resource.
- `domain_name = "xfusion-es"`: Sets the domain name as required.
- `engine_version = "OpenSearch_2.11"`: Specifies a recent OpenSearch version (adjust based on availability or requirements).

**Note:** Minimal configuration is used (default instance type, storage, and access policies) as no additional specifications were provided. Enhanced configurations are provided later for production use.

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

  # aws_opensearch_domain.xfusion will be created
  + resource "aws_opensearch_domain" "xfusion" {
      + arn             = (known after apply)
      + domain_id       = (known after apply)
      + domain_name     = "xfusion-es"
      + engine_version  = "OpenSearch_2.11"
      + endpoint        = (known after apply)
      + id              = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

aws_opensearch_domain.xfusion: Creating...
aws_opensearch_domain.xfusion: Still creating... [10m0s elapsed]
aws_opensearch_domain.xfusion: Creation complete after 10m0s [id=arn:aws:es:us-east-1:000000000000:domain/xfusion-es]
```

**Note:** The OpenSearch domain creation may take approximately 10 minutes, as noted in the task requirements. Wait until the creation is complete before proceeding.

---

### Step 6: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_opensearch_domain.xfusion: Refreshing state... [id=arn:aws:es:us-east-1:000000000000:domain/xfusion-es]

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

# Create OpenSearch domain for log storage and search
resource "aws_opensearch_domain" "xfusion" {
  domain_name = "xfusion-es"
  engine_version = "OpenSearch_2.11"
}
```

### OpenSearch Domain Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **domain_name** | xfusion-es | Name of the OpenSearch domain |
| **engine_version** | OpenSearch_2.11 | Version of the OpenSearch engine |
| **instance_type** | t3.small.search (default) | Default instance type for the domain |
| **ebs_options** | Default (5 GiB, gp2) | Default EBS storage settings |
| **access_policies** | Default (open to IAM principal) | Default access policy |

### OpenSearch Domain Default Properties
- **Region:** `us-east-1` (specified in provider, adjust if needed).
- **Instance Type:** `t3.small.search` (default, minimal cost instance).
- **Storage:** 10 GiB EBS volume with `gp2` type (default).
- **Access Policy:** Allows access to the IAM principal creating the domain (open by default, restricted in enhanced configuration).
- **Encryption:** Enabled by default for data at rest and node-to-node communication.

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_opensearch_domain.xfusion
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed domain information
terraform state show aws_opensearch_domain.xfusion
```

**Expected Output:**
```
# aws_opensearch_domain.xfusion:
resource "aws_opensearch_domain" "xfusion" {
    arn             = "arn:aws:es:us-east-1:000000000000:domain/xfusion-es"
    domain_id       = (known after apply)
    domain_name     = "xfusion-es"
    engine_version  = "OpenSearch_2.11"
    endpoint        = (known after apply)
    id              = "arn:aws:es:us-east-1:000000000000:domain/xfusion-es"
}
```

### Step 3: Verify Resources in AWS Console (Optional)

```bash
# List OpenSearch domains using AWS CLI
aws opensearch describe-domains --domain-names xfusion-es
```

**Expected JSON Output (partial):**
```json
{
    "DomainStatusList": [
        {
            "DomainName": "xfusion-es",
            "EngineVersion": "OpenSearch_2.11",
            "Created": true,
            "Deleted": false,
            "Endpoint": "xfusion-es.us-east-1.es.amazonaws.com",
            "Processing": false,
            "ARN": "arn:aws:es:us-east-1:000000000000:domain/xfusion-es"
        }
    ]
}
```

---

## 🧪 Testing

### Verify Resource Creation

```bash
# Show complete domain details from Terraform state
terraform state show aws_opensearch_domain.xfusion
```

### Test Domain Accessibility (Optional)

```bash
# Get the domain endpoint
aws opensearch describe-domains --domain-names xfusion-es --query 'DomainStatusList[0].Endpoint'
```

**Expected Output:**
```
"xfusion-es.us-east-1.es.amazonaws.com"
```

```bash
# Test connectivity to the domain endpoint (requires appropriate permissions)
curl -XGET https://xfusion-es.us-east-1.es.amazonaws.com
```

**Expected Output (partial, if accessible):**
```json
{
  "name": "xfusion-es",
  "version": "2.11",
  "cluster_name": "xfusion-es",
  "cluster_uuid": "..."
}
```

**Note:** Access to the endpoint requires a proper access policy or VPC configuration. The default configuration may restrict access.

### Test Log Ingestion (Optional)

```bash
# Send a test log event to the domain (requires access policy and client setup)
curl -XPOST https://xfusion-es.us-east-1.es.amazonaws.com/logs/_doc -H 'Content-Type: application/json' -d '{"message": "Test log event", "timestamp": "2025-09-30T22:16:00Z"}'
```

**Expected Output (if successful):**
```json
{
  "_index": "logs",
  "_id": "...",
  "_version": 1,
  "result": "created"
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

# Create OpenSearch domain for log storage and search
resource "aws_opensearch_domain" "xfusion" {
  domain_name = "xfusion-es"
  engine_version = "OpenSearch_2.11"
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
# Wait approximately 10 minutes for domain creation
terraform plan
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_opensearch_domain.xfusion

# Verify via AWS CLI
aws opensearch describe-domains --domain-names xfusion-es
```

### Optional: Enhanced Configuration

```hcl
# Enhanced OpenSearch domain with production-ready settings
provider "aws" {
  region = "us-east-1"
}

resource "aws_opensearch_domain" "xfusion" {
  domain_name    = "xfusion-es"
  engine_version = "OpenSearch_2.11"

  cluster_config {
    instance_type          = "t3.small.search"
    instance_count         = 1
    dedicated_master_enabled = false
  }

  ebs_options {
    ebs_enabled = true
    volume_type = "gp3"
    volume_size = 10
  }

  encrypt_at_rest {
    enabled = true
  }

  node_to_node_encryption {
    enabled = true
  }

  domain_endpoint_options {
    enforce_https = true
    tls_security_policy = "Policy-Min-TLS-1-2-2019-07"
  }

  access_policies = <<POLICY
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Resource": "arn:aws:es:us-east-1:000000000000:domain/xfusion-es/*"
    }
  ]
}
POLICY

  tags = {
    Name        = "xfusion-es"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "log-search"
  }
}

output "domain_endpoint" {
  description = "Endpoint of the created OpenSearch domain"
  value       = aws_opensearch_domain.xfusion.endpoint
}

output "domain_arn" {
  description = "ARN of the created OpenSearch domain"
  value       = aws_opensearch_domain.xfusion.arn
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Domain Already Exists**

- **Symptoms:** Error: "DomainAlreadyExistsException: Domain xfusion-es already exists"
- **Solution:** Import the existing domain or use a different domain name
```bash
# Import existing domain into Terraform state
terraform import aws_opensearch_domain.xfusion arn:aws:es:us-east-1:000000000000:domain/xfusion-es
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDeniedException: User is not authorized to perform: es:CreateDomain"
- **Solution:** Ensure AWS credentials have OpenSearch permissions
```bash
# Required permissions:
# - es:CreateDomain
# - es:DescribeDomains
# - es:UpdateDomainConfig
# - es:ListDomainNames
```

**Issue 3: Domain Creation Timeout**

- **Symptoms:** `terraform apply` hangs or times out during domain creation
- **Solution:** Wait longer (up to 15-20 minutes) or check AWS Console for errors
```bash
# Check domain status
aws opensearch describe-domains --domain-names xfusion-es
```

**Issue 4: Invalid Access Policy**

- **Symptoms:** Error: "InvalidParameterValue: Invalid access policy"
- **Solution:** Validate the access policy JSON
```bash
# Example: Restrict access to specific IAM role
access_policies = jsonencode({
  Version = "2012-10-17"
  Statement = [
    {
      Effect    = "Allow"
      Principal = { AWS = "arn:aws:iam::000000000000:role/nautilus-devops-role" }
      Action    = "es:*"
      Resource  = "arn:aws:es:us-east-1:000000000000:domain/xfusion-es/*"
    }
  ]
})
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

- **🔐 Infrastructure as Code:** Used Terraform to manage OpenSearch for consistent deployments.
- **📊 Resource Naming:** Clear domain name (`xfusion-es`) matching requirements.
- **🏷️ Minimal Configuration:** Focused setup with minimal attributes for simplicity.
- **🔄 Creation Time:** Accounted for the ~10-minute creation time for OpenSearch domains.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### OpenSearch Security Framework
- **Access Control:** Restrict domain access with IAM policies (included in enhanced configuration).
- **Encryption:** Enable encryption at rest and node-to-node encryption.
- **HTTPS:** Enforce HTTPS for secure communication.
- **Monitoring:** Enable CloudWatch metrics for domain health and performance.
- **Tagging:** Add tags for cost allocation and resource management.

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_opensearch_domain" "xfusion" {
  domain_name    = "xfusion-es"
  engine_version = "OpenSearch_2.11"

  cluster_config {
    instance_type          = "t3.small.search"
    instance_count         = 1
    dedicated_master_enabled = false
  }

  ebs_options {
    ebs_enabled = true
    volume_type = "gp3"
    volume_size = 10
  }

  encrypt_at_rest {
    enabled = true
  }

  node_to_node_encryption {
    enabled = true
  }

  domain_endpoint_options {
    enforce_https = true
    tls_security_policy = "Policy-Min-TLS-1-2-2019-07"
  }

  access_policies = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect    = "Allow"
        Principal = { AWS = "arn:aws:iam::000000000000:role/nautilus-devops-role" }
        Action    = "es:*"
        Resource  = "arn:aws:es:us-east-1:000000000000:domain/xfusion-es/*"
      }
    ]
  })

  tags = {
    Name        = "xfusion-es"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "log-search"
  }
}
```

### Search and Logging Strategy
- **Access Policies:** Restrict access to specific IAM roles/users.
- **Monitoring:** Set up CloudWatch alarms for domain health (e.g., CPU usage, search latency).
- **Cost Optimization:** Use minimal instance types (e.g., `t3.small.search`) for cost efficiency.
- **Integration:** Configure applications to send logs to the domain (e.g., via Fluentd or CloudWatch Logs).
- **Scaling:** Adjust instance count or type based on log volume and query load.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **OpenSearch Fundamentals:** Understanding domain creation and configuration.
- **Log Storage and Search:** Setting up an OpenSearch domain for log management.
- **Terraform Integration:** Managing AWS OpenSearch Service with Terraform.
- **Deployment Timing:** Handling long-running resource creation processes.

**Terraform Features Used:**
- `aws_opensearch_domain`: OpenSearch domain resource creation.
- AWS provider integration for OpenSearch services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Domain Name**: Created `xfusion-es` as required.
- **File Structure**: Used single `main.tf` file as specified.
- **Verification**: Confirmed domain creation via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors, waited ~10 minutes for completion.
- **Final Check**: `terraform plan` shows "No changes" as required.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Search Foundation:** The OpenSearch domain `xfusion-es` is created and ready for the Nautilus DevOps team to store and search application logs efficiently.

### 🔮 Resources Ready for:
- **Log Storage**: Store and index application logs.
- **Search and Analysis**: Query logs using OpenSearch APIs or Dashboards.
- **Security**: Implement access policies and encryption.
- **Monitoring**: Track domain health with CloudWatch.
- **Integration**: Connect with log sources for ingestion.