```markdown
# 🌟 Task 2 - AWS Security Group Creation with Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- The name of the security group must be **`datacenter-sg`**
- The description must be **`Security group for Nautilus App Servers`**
- Add an inbound rule of type **HTTP**, with port range **80**, and source CIDR range **0.0.0.0/0**
- Add another inbound rule of type **SSH**, with port range **22**, and source CIDR range **0.0.0.0/0**
- Ensure the security group is created in the **us-east-1** region
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create a security group under the default VPC using Terraform with specified inbound rules for HTTP and SSH access.

💡 **Note:** Use the default VPC for this security group configuration.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- AWS Security Group in default VPC
- HTTP inbound rule (port 80)
- SSH inbound rule (port 22)
- Default egress rules

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Data Source:** Retrieves default VPC information for security group placement
- **Security Group Resource:** Creates the main security group with proper naming and description
- **Ingress Rules:** HTTP (port 80) and SSH (port 22) access from anywhere (0.0.0.0/0)
- **Egress Rules:** Default outbound access to all destinations

### 🎯 Implementation Strategy
1. Query the default VPC using data source
2. Create security group with required name and description
3. Configure HTTP ingress rule for web traffic
4. Configure SSH ingress rule for remote access
5. Add default egress rule for outbound traffic

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

# Data source to get the default VPC
data "aws_vpc" "default" {
  default = true
}

# Create security group for Nautilus App Servers
resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  # HTTP inbound rule
  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # SSH inbound rule
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Default egress rule (outbound traffic)
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Tags for resource identification
  tags = {
    Name = "datacenter-sg"
  }
}
```

**Configuration Breakdown:**
- `data "aws_vpc"` - Retrieves the default VPC for the region
- `aws_security_group` - Creates security group with specified name and description
- `ingress` blocks - Define inbound rules for HTTP and SSH traffic
- `egress` block - Allows all outbound traffic (default behavior)
- `tags` - Proper resource tagging for identification

---

### Step 3: Initialize Terraform
```bash
terraform init
```

**Purpose:** Initialize the Terraform working directory and download required AWS provider.

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

  # aws_security_group.datacenter_sg will be created
  + resource "aws_security_group" "datacenter_sg" {
      + arn                    = (known after apply)
      + description            = "Security group for Nautilus App Servers"
      + egress                 = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + from_port        = 0
              + protocol         = "-1"
              + to_port          = 0
            },
        ]
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = "Allow HTTP"
              + from_port        = 80
              + protocol         = "tcp"
              + to_port          = 80
            },
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = "Allow SSH"
              + from_port        = 22
              + protocol         = "tcp"
              + to_port          = 22
            },
        ]
      + name                   = "datacenter-sg"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags                   = {
          + "Name" = "datacenter-sg"
        }
      + tags_all               = {
          + "Name" = "datacenter-sg"
        }
      + vpc_id                 = "vpc-xxxxxxxxx"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Data source to get the default VPC
data "aws_vpc" "default" {
  default = true                    # Filter for the default VPC in the region
}

# Create security group for Nautilus App Servers
resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-sg"                           # Required security group name
  description = "Security group for Nautilus App Servers" # Required description
  vpc_id      = data.aws_vpc.default.id                   # Associate with default VPC

  # HTTP inbound rule - Allow web traffic
  ingress {
    description = "Allow HTTP"      # Rule description for clarity
    from_port   = 80               # HTTP port
    to_port     = 80               # Single port rule
    protocol    = "tcp"            # TCP protocol for HTTP
    cidr_blocks = ["0.0.0.0/0"]    # Allow from anywhere
  }

  # SSH inbound rule - Allow remote access
  ingress {
    description = "Allow SSH"       # Rule description for clarity
    from_port   = 22               # SSH port
    to_port     = 22               # Single port rule
    protocol    = "tcp"            # TCP protocol for SSH
    cidr_blocks = ["0.0.0.0/0"]    # Allow from anywhere
  }

  # Default egress rule - Allow all outbound traffic
  egress {
    from_port   = 0                # All ports
    to_port     = 0                # All ports
    protocol    = "-1"             # All protocols
    cidr_blocks = ["0.0.0.0/0"]    # To anywhere
  }

  # Resource tags for identification and management
  tags = {
    Name = "datacenter-sg"
  }
}
```

### Resource Dependencies
- `aws_security_group` depends on `data.aws_vpc.default` for VPC ID
- Data source automatically queries the default VPC in the current region
- Terraform handles the dependency order automatically

---

## ✅ Verification Steps

### Step 1: Verify Security Group Creation
```bash
# List Terraform state to confirm resources
terraform state list
```

**Expected Output:**
```
data.aws_vpc.default
aws_security_group.datacenter_sg
```

### Step 2: Check Security Group Details
```bash
# Show detailed information about created resources
terraform show
```

### Step 3: Verify Security Group in AWS Console
```bash
# Get security group ID for verification
terraform output -raw security_group_id
```

**Note:** Add output block if needed for security group ID reference.

---

## 🧪 Testing

### Verify Security Group Rules
```bash
# Show security group details from Terraform state
terraform state show aws_security_group.datacenter_sg
```

### Test Security Group Access (Optional)
```bash
# AWS CLI command to describe the security group
aws ec2 describe-security-groups --group-names datacenter-sg --region us-east-1
```

**Expected JSON Output:**
```json
{
    "SecurityGroups": [
        {
            "GroupName": "datacenter-sg",
            "Description": "Security group for Nautilus App Servers",
            "IpPermissions": [
                {
                    "FromPort": 22,
                    "ToPort": 22,
                    "IpProtocol": "tcp",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}]
                },
                {
                    "FromPort": 80,
                    "ToPort": 80,
                    "IpProtocol": "tcp",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}]
                }
            ]
        }
    ]
}
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Data source to get the default VPC
data "aws_vpc" "default" {
  default = true
}

# Create security group for Nautilus App Servers
resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  # HTTP inbound rule
  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # SSH inbound rule
  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Default egress rule
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Resource tags
  tags = {
    Name = "datacenter-sg"
  }
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
terraform state list
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Default VPC not found**
- **Symptoms:** Error: "No matching VPC found"
- **Solution:** Verify default VPC exists in us-east-1 region
```bash
# Check if default VPC exists
aws ec2 describe-vpcs --filters "Name=is-default,Values=true" --region us-east-1
```

**Issue 2: Security group name already exists**
- **Symptoms:** Error: "InvalidGroup.Duplicate"
- **Solution:** Import existing security group or use different name
```bash
# Import existing security group
terraform import aws_security_group.datacenter_sg sg-xxxxxxxxx
```

**Issue 3: Insufficient permissions**
- **Symptoms:** Error: "UnauthorizedOperation"
- **Solution:** Ensure AWS credentials have required EC2 permissions
```bash
# Check current AWS identity
aws sts get-caller-identity
# Required permissions: ec2:CreateSecurityGroup, ec2:DescribeVpcs
```

**Issue 4: Region mismatch**
- **Symptoms:** Resources created in wrong region
- **Solution:** Set AWS region explicitly
```bash
# Set region via environment variable
export AWS_DEFAULT_REGION=us-east-1
# Or configure in AWS provider block
```

---

## 💡 Best Practices Applied

- **🔐 Security:** Explicit ingress rules with clear descriptions
- **📊 Resource Management:** Using data sources to reference existing infrastructure
- **🏷️ Naming Conventions:** Descriptive names matching requirements
- **🔄 State Management:** Proper resource tagging for identification
- **📍 Region Awareness:** Implicit region handling via AWS provider configuration

---

## 🚀 Production Considerations

- **Security:** Consider restricting SSH access to specific IP ranges instead of 0.0.0.0/0
- **Monitoring:** Implement VPC Flow Logs to monitor security group traffic
- **Access Control:** Use IAM policies to control security group modifications
- **Backup/Recovery:** Security groups are automatically backed up by AWS
- **Compliance:** Document security group rules for compliance requirements

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **AWS Data Sources:** Using data sources to query existing AWS resources
- **Security Group Management:** Creating and configuring AWS security groups with Terraform
- **Network Security:** Understanding inbound and outbound traffic rules

**Terraform Features Used:**
- `data "aws_vpc"` - Data source for VPC information
- `aws_security_group` - Security group resource creation
- `ingress` and `egress` blocks - Traffic rule configuration
- Resource dependencies and references

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Security Group Name** - Created "datacenter-sg" as required
- **Description** - Set to "Security group for Nautilus App Servers"
- **HTTP Rule** - Configured inbound HTTP (port 80) from 0.0.0.0/0
- **SSH Rule** - Configured inbound SSH (port 22) from 0.0.0.0/0
- **VPC Placement** - Deployed in default VPC as specified
- **Region** - Created in us-east-1 region
- **File Structure** - Used single `main.tf` file as required

**Final Status:** 🎉 **Task completed successfully with all requirements met**

The AWS security group is now ready to be attached to EC2 instances for the Nautilus application servers, providing secure HTTP and SSH access as part of the incremental cloud migration strategy.
```