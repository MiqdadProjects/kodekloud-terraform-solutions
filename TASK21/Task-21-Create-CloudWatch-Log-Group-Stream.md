# 🌟 Task 21 - Create AWS CloudWatch Log Group and Stream Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** needs to set up AWS CloudWatch logging to capture and store application logs for monitoring and troubleshooting. They require a CloudWatch log group and log stream to organize and manage log data effectively.

**Requirements:**
- Create a CloudWatch log group named **`nautilus-log-group`** using Terraform.
- Create a log stream named **`nautilus-log-stream`** within the log group.
- The Terraform working directory is **`/home/bob/terraform`**.
- Create the **`main.tf`** file (do not create a different `.tf` file).

👉 **Your task:** Create a CloudWatch log group and log stream using Terraform to enable logging capabilities for the Nautilus DevOps team’s applications.

💡 **Note:** AWS CloudWatch Logs is a service for storing, monitoring, and analyzing log data. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable via Terraform and AWS CLI. The current date and time is September 29, 2025, 11:17 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (CloudWatch Logs, region-specific)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- CloudWatch log group for organizing log data
- CloudWatch log stream for storing log events
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **CloudWatch Log Group Resource:** Creates a container for log streams.
- **Log Group Name:** Configures the log group as `nautilus-log-group`.
- **CloudWatch Log Stream Resource:** Creates a stream for log events within the log group.
- **Log Stream Name:** Configures the log stream as `nautilus-log-stream`.
- **Logging Framework:** Prepares for application log storage and monitoring.

### 🎯 Implementation Strategy
1. Create a CloudWatch log group with the name `nautilus-log-group`.
2. Create a log stream named `nautilus-log-stream` within the log group.
3. Deploy the resources using Terraform in the specified directory.
4. Verify the creation of both the log group and log stream using Terraform state and AWS CLI.

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

# Create CloudWatch log group for organizing log data
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
  name = "nautilus-log-group"
}

# Create CloudWatch log stream for storing log events
resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
  name           = "nautilus-log-stream"
  log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name
}
```

**Configuration Breakdown:**
- `aws_cloudwatch_log_group`: Defines a CloudWatch log group resource.
- `name = "nautilus-log-group"`: Sets the log group name as required.
- `aws_cloudwatch_log_stream`: Defines a CloudWatch log stream resource.
- `name = "nautilus-log-stream"`: Sets the log stream name as required.
- `log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name`: Associates the log stream with the log group.

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

  # aws_cloudwatch_log_group.nautilus_log_group will be created
  + resource "aws_cloudwatch_log_group" "nautilus_log_group" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + name             = "nautilus-log-group"
      + tags_all         = (known after apply)
    }

  # aws_cloudwatch_log_stream.nautilus_log_stream will be created
  + resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + log_group_name   = "nautilus-log-group"
      + name             = "nautilus-log-stream"
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
aws_cloudwatch_log_group.nautilus_log_group: Refreshing state... [id=nautilus-log-group]
aws_cloudwatch_log_stream.nautilus_log_stream: Refreshing state... [id=nautilus-log-stream]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Create CloudWatch log group for organizing log data
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
  name = "nautilus-log-group"
}

# Create CloudWatch log stream for storing log events
resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
  name           = "nautilus-log-stream"
  log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name
}
```

### CloudWatch Log Group and Stream Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **Log Group: name** | nautilus-log-group | The name of the CloudWatch log group |
| **Log Group: retention_in_days** | null | Default (logs retained indefinitely) |
| **Log Group: tags** | {} | No tags defined (optional for future use) |
| **Log Stream: name** | nautilus-log-stream | The name of the log stream |
| **Log Stream: log_group_name** | nautilus-log-group | The associated log group |

### CloudWatch Log Group and Stream Default Properties
- **Retention Period:** Indefinite by default (logs not deleted unless specified).
- **Encryption:** Not enabled by default (can use KMS for encryption).
- **Access Policy:** Controlled by IAM permissions for the AWS account.
- **Log Stream Dependency:** The log stream is tied to the log group via `log_group_name`.

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_cloudwatch_log_group.nautilus_log_group
aws_cloudwatch_log_stream.nautilus_log_stream
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed log group information
terraform state show aws_cloudwatch_log_group.nautilus_log_group
```

**Expected Output:**
```
# aws_cloudwatch_log_group.nautilus_log_group:
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
    arn              = "arn:aws:logs:us-east-1:000000000000:log-group:nautilus-log-group:*"
    id               = "nautilus-log-group"
    name             = "nautilus-log-group"
    tags_all         = {}
}
```

```bash
# Show detailed log stream information
terraform state show aws_cloudwatch_log_stream.nautilus_log_stream
```

**Expected Output:**
```
# aws_cloudwatch_log_stream.nautilus_log_stream:
resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
    arn              = "arn:aws:logs:us-east-1:000000000000:log-group:nautilus-log-group:log-stream:nautilus-log-stream"
    id               = "nautilus-log-stream"
    log_group_name   = "nautilus-log-group"
    name             = "nautilus-log-stream"
}
```

### Step 3: Verify Resources in AWS Console (Optional)

```bash
# List CloudWatch log groups using AWS CLI
aws logs describe-log-groups --query 'logGroups[?logGroupName==`nautilus-log-group`]'
```

**Expected JSON Output:**
```json
[
    {
        "logGroupName": "nautilus-log-group",
        "creationTime": 1759326240000,
        "arn": "arn:aws:logs:us-east-1:000000000000:log-group:nautilus-log-group:*"
    }
]
```

```bash
# List log streams in the log group
aws logs describe-log-streams --log-group-name nautilus-log-group --query 'logStreams[?logStreamName==`nautilus-log-stream`]'
```

**Expected JSON Output:**
```json
[
    {
        "logStreamName": "nautilus-log-stream",
        "creationTime": 1759326240000,
        "arn": "arn:aws:logs:us-east-1:000000000000:log-group:nautilus-log-group:log-stream:nautilus-log-stream"
    }
]
```

---

## 🧪 Testing

### Verify Resource Creation

```bash
# Show complete resource details from Terraform state
terraform state show aws_cloudwatch_log_group.nautilus_log_group
terraform state show aws_cloudwatch_log_stream.nautilus_log_stream
```

### Test Resource Existence via AWS CLI

```bash
# Describe the log group to confirm configuration
aws logs describe-log-groups --log-group-name-prefix nautilus-log-group
```

**Expected Output (partial):**
```json
{
    "logGroups": [
        {
            "logGroupName": "nautilus-log-group",
            "creationTime": 1759326240000,
            "arn": "arn:aws:logs:us-east-1:000000000000:log-group:nautilus-log-group:*"
        }
    ]
}
```

```bash
# Describe the log stream to confirm configuration
aws logs describe-log-streams --log-group-name nautilus-log-group --log-stream-name-prefix nautilus-log-stream
```

**Expected Output (partial):**
```json
{
    "logStreams": [
        {
            "logStreamName": "nautilus-log-stream",
            "creationTime": 1759326240000,
            "arn": "arn:aws:logs:us-east-1:000000000000:log-group:nautilus-log-group:log-stream:nautilus-log-stream"
        }
    ]
}
```

### Test Log Stream Accessibility (Optional)

```bash
# Attempt to put a test log event (requires appropriate permissions)
aws logs put-log-events --log-group-name nautilus-log-group --log-stream-name nautilus-log-stream --log-events '[{"timestamp": 1759326240000, "message": "Test log event"}]'
```

**Expected Output (partial):**
```json
{
    "nextSequenceToken": "49656789123456789012345678901234567890123456789012345"
}
```

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Create CloudWatch log group for organizing log data
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
  name = "nautilus-log-group"
}

# Create CloudWatch log stream for storing log events
resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
  name           = "nautilus-log-stream"
  log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name
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
aws logs describe-log-groups --log-group-name-prefix nautilus-log-group
aws logs describe-log-streams --log-group-name nautilus-log-group --log-stream-name-prefix nautilus-log-stream
```

### Optional: Enhanced Configuration

```hcl
# Enhanced CloudWatch log group and stream with additional features
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
  name              = "nautilus-log-group"
  retention_in_days = 7  # Retain logs for 7 days

  tags = {
    Name        = "nautilus-log-group"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "application-logging"
  }
}

resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
  name           = "nautilus-log-stream"
  log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name
}

# Optional: Add outputs for easy reference
output "log_group_name" {
  description = "Name of the created CloudWatch log group"
  value       = aws_cloudwatch_log_group.nautilus_log_group.name
}

output "log_group_arn" {
  description = "ARN of the created CloudWatch log group"
  value       = aws_cloudwatch_log_group.nautilus_log_group.arn
}

output "log_stream_name" {
  description = "Name of the created CloudWatch log stream"
  value       = aws_cloudwatch_log_stream.nautilus_log_stream.name
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Log Group Already Exists**

- **Symptoms:** Error: "ResourceAlreadyExistsException: The specified log group already exists"
- **Solution:** Import the existing log group or use a different name
```bash
# Import existing log group into Terraform state
terraform import aws_cloudwatch_log_group.nautilus_log_group nautilus-log-group
```

**Issue 2: Log Stream Already Exists**

- **Symptoms:** Error: "ResourceAlreadyExistsException: The specified log stream already exists"
- **Solution:** Import the existing log stream or use a different name
```bash
# Import existing log stream into Terraform state
terraform import aws_cloudwatch_log_stream.nautilus_log_stream nautilus-log-group/nautilus-log-stream
```

**Issue 3: Insufficient Permissions**

- **Symptoms:** Error: "AccessDeniedException" or "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have CloudWatch Logs permissions
```bash
# Required permissions:
# - logs:CreateLogGroup
# - logs:CreateLogStream
# - logs:DescribeLogGroups
# - logs:DescribeLogStreams
# - logs:TagResource (if using tags)
```

**Issue 4: Invalid Log Group or Stream Name**

- **Symptoms:** Error: "InvalidParameterException: Invalid log group/stream name"
- **Solution:** Ensure names meet AWS CloudWatch Logs naming requirements
```bash
# Valid characters: A-Z, a-z, 0-9, /, -, _, .
# Log group name max length: 512 characters
# Log stream name max length: 512 characters
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

- **🔐 Logging Framework:** Simple log group and stream setup for flexible logging.
- **📊 Resource Naming:** Clear, descriptive names (`nautilus-log-group`, `nautilus-log-stream`) matching requirements.
- **🏷️ Minimal Configuration:** Focused setup with only required attributes for simplicity.
- **🔄 Dependency Management:** Log stream correctly references log group.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### CloudWatch Logs Security Framework
- **Encryption:** Enable KMS encryption for log data security (optional in enhanced configuration).
- **Access Control:** Restrict log group/stream access with IAM policies.
- **Monitoring:** Enable CloudWatch metrics for log event tracking.
- **Retention:** Set a retention period (e.g., 7 days) to manage storage costs.
- **Tagging:** Add tags for cost allocation and resource management.

### Enhanced Configuration

```hcl
# Production-ready CloudWatch log group and stream with additional features
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
  name              = "nautilus-log-group"
  retention_in_days = 7  # Retain logs for 7 days
  kms_key_id        = "alias/aws/logs"  # Requires valid KMS key

  tags = {
    Name        = "nautilus-log-group"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "application-logging"
  }
}

resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
  name           = "nautilus-log-stream"
  log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name
}
```

### Logging Strategy
- **Retention Policy:** Set retention period to balance storage costs and log availability.
- **Access Policies:** Restrict log writing/reading with IAM policies.
- **Monitoring:** Set up CloudWatch alarms for log event anomalies.
- **Integration:** Configure applications (e.g., Lambda, EC2) to send logs to the stream.
- **Cost Optimization:** Monitor log storage and ingestion costs.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **CloudWatch Logs Fundamentals:** Understanding log group and stream configuration.
- **Log Organization:** Setting up a log group and stream for structured logging.
- **Terraform Integration:** Managing AWS CloudWatch Logs resources with Terraform.
- **Dependency Management:** Ensuring proper resource dependencies in Terraform.

**Terraform Features Used:**
- `aws_cloudwatch_log_group`: Log group resource creation.
- `aws_cloudwatch_log_stream`: Log stream resource creation.
- Resource dependency management via `log_group_name`.
- AWS provider integration for logging services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Log Group Name**: Created `nautilus-log-group` as required.
- **Log Stream Name**: Created `nautilus-log-stream` within the log group.
- **File Structure**: Used single `main.tf` file as specified.
- **Verification**: Confirmed resource creation via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Logging Foundation:** The CloudWatch log group `nautilus-log-group` and log stream `nautilus-log-stream` are now created and ready for application logging, supporting the Nautilus DevOps team’s monitoring and troubleshooting needs.

### 🔮 Resources Ready for:
- **Log Storage**: Store application logs in the log stream.
- **Monitoring**: Analyze logs with CloudWatch Logs Insights.
- **Security**: Implement encryption and IAM policies for secure logging.
- **Integration**: Connect with applications for log ingestion.
- **Cost Management**: Configure retention for cost-effective storage.
