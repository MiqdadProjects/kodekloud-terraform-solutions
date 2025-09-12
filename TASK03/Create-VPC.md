```markdown
# 🌟 Task 3 - Create VPC Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- Create a VPC named **`nautilus-vpc`**
- Deploy in region **`us-east-1`**
- Use any IPv4 CIDR block
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create a Virtual Private Cloud (VPC) using Terraform to establish the foundational network infrastructure for the Nautilus application migration.

💡 **Note:** This VPC will serve as the isolated network environment for future AWS resources in the migration process.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- AWS VPC with custom IPv4 CIDR block
- Network isolation for application resources
- Foundation for subnet and security group deployment

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **VPC Resource:** Creates an isolated virtual network in AWS
- **IPv4 CIDR Block:** Defines the IP address range for the VPC (10.0.0.0/16)
- **Resource Tagging:** Proper naming convention for resource identification
- **Regional Deployment:** Deployed specifically in us-east-1 region

### 🎯 Implementation Strategy
1. Define VPC resource with appropriate CIDR block
2. Configure proper naming through tags
3. Use standard private IP range for internal networking
4. Ensure resource is properly tagged for management

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

# Create VPC for Nautilus infrastructure
resource "aws_vpc" "nautilus_vpc" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "nautilus-vpc"
  }
}
```

**Configuration Breakdown:**
- `aws_vpc` - Creates a Virtual Private Cloud in AWS
- `cidr_block = "10.0.0.0/16"` - Defines IP address range (65,536 IP addresses)
- `tags` - Assigns the required name "nautilus-vpc" for identification

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

  # aws_vpc.nautilus_vpc will be created
  + resource "aws_vpc" "nautilus_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                          = "10.0.0.0/16"
      + default_network_acl_id              = (known after apply)
      + default_route_table_id              = (known after apply)
      + default_security_group_id           = (known after apply)
      + dhcp_options_id                     = (known after apply)
      + enable_dns_hostnames                = false
      + enable_dns_support                  = true
      + enable_network_address_usage_metrics = false
      + id                                  = (known after apply)
      + instance_tenancy                    = "default"
      + ipv6_association_id                 = (known after apply)
      + ipv6_cidr_block                     = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                 = (known after apply)
      + owner_id                            = (known after apply)
      + tags                                = {
          + "Name" = "nautilus-vpc"
        }
      + tags_all                            = {
          + "Name" = "nautilus-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create VPC for Nautilus infrastructure
resource "aws_vpc" "nautilus_vpc" {
  cidr_block = "10.0.0.0/16"          # IPv4 CIDR block for the VPC
                                       # Provides 65,536 IP addresses (10.0.0.1 - 10.0.255.254)
  
  tags = {
    Name = "nautilus-vpc"              # Required name tag for resource identification
  }
}
```

### CIDR Block Analysis
- **10.0.0.0/16:** Private IP range suitable for internal networking
- **Address Range:** 10.0.0.0 to 10.0.255.255
- **Usable IPs:** 65,534 (excluding network and broadcast addresses)
- **Subnet Possibilities:** Can be divided into multiple /24 subnets (256 IPs each)

### Default VPC Attributes
When created, the VPC automatically gets:
- **DNS Support:** Enabled by default
- **DNS Hostnames:** Disabled by default  
- **Default Route Table:** Created automatically
- **Default Network ACL:** Created automatically
- **Default Security Group:** Created automatically

---

## ✅ Verification Steps

### Step 1: Verify VPC Creation
```bash
# List Terraform state to confirm resources
terraform state list
```

**Expected Output:**
```
aws_vpc.nautilus_vpc
```

### Step 2: Check VPC Details
```bash
# Show detailed VPC information
terraform show
```

### Step 3: Verify VPC in AWS Console
```bash
# Get VPC ID for verification
terraform state show aws_vpc.nautilus_vpc | grep "id"
```

**Expected Output:**
```
id = "vpc-xxxxxxxxxxxxxxxxx"
```

---

## 🧪 Testing

### Verify VPC Properties
```bash
# Show VPC details from Terraform state
terraform state show aws_vpc.nautilus_vpc
```

### AWS CLI Verification
```bash
# Describe the created VPC using AWS CLI
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=nautilus-vpc" --region us-east-1
```

**Expected JSON Output:**
```json
{
    "Vpcs": [
        {
            "CidrBlock": "10.0.0.0/16",
            "DhcpOptionsId": "dopt-xxxxxxxxx",
            "State": "available",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "OwnerId": "xxxxxxxxxxxx",
            "InstanceTenancy": "default",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "nautilus-vpc"
                }
            ]
        }
    ]
}
```

### Test VPC Connectivity
```bash
# Check default route table
aws ec2 describe-route-tables --filters "Name=vpc-id,Values=$(terraform output -raw vpc_id)" --region us-east-1

# Check default security group
aws ec2 describe-security-groups --filters "Name=vpc-id,Values=$(terraform output -raw vpc_id)" --region us-east-1
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create VPC for Nautilus infrastructure
resource "aws_vpc" "nautilus_vpc" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "nautilus-vpc"
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

### Optional: Add Output for VPC ID
```hcl
# Add to main.tf for easy reference
output "vpc_id" {
  description = "ID of the created VPC"
  value       = aws_vpc.nautilus_vpc.id
}

output "vpc_cidr_block" {
  description = "CIDR block of the VPC"
  value       = aws_vpc.nautilus_vpc.cidr_block
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: CIDR block conflicts**
- **Symptoms:** Error: "InvalidVpc.Range: The CIDR 'x.x.x.x/x' conflicts with another subnet"
- **Solution:** Choose a different CIDR block or check existing VPCs
```bash
# List existing VPCs and their CIDR blocks
aws ec2 describe-vpcs --query 'Vpcs[*].[VpcId,CidrBlock,State]' --output table
```

**Issue 2: VPC limit exceeded**
- **Symptoms:** Error: "VpcLimitExceeded: The maximum number of VPCs has been reached"
- **Solution:** Delete unused VPCs or request limit increase
```bash
# Check VPC usage
aws ec2 describe-vpcs --query 'length(Vpcs)'
# Default limit is 5 VPCs per region
```

**Issue 3: Insufficient permissions**
- **Symptoms:** Error: "UnauthorizedOperation: You are not authorized to perform this operation"
- **Solution:** Ensure AWS credentials have EC2 VPC permissions
```bash
# Required permissions:
# - ec2:CreateVpc
# - ec2:DescribeVpcs
# - ec2:CreateTags
```

**Issue 4: Region mismatch**
- **Symptoms:** VPC created in wrong region
- **Solution:** Explicitly set AWS region
```bash
# Set region via environment variable
export AWS_DEFAULT_REGION=us-east-1

# Or add provider block to main.tf
provider "aws" {
  region = "us-east-1"
}
```

---

## 💡 Best Practices Applied

- **🔐 Network Security:** Using private IP range (10.0.0.0/16) for internal communication
- **📊 Resource Management:** Proper resource tagging for identification and billing
- **🏷️ Naming Conventions:** Clear, descriptive naming following requirements
- **🔄 CIDR Planning:** Chose /16 to allow for multiple subnets and scalability
- **📍 Regional Strategy:** Explicit regional deployment in us-east-1

---

## 🚀 Production Considerations

- **Scalability:** /16 CIDR block allows for 256 /24 subnets
- **Multi-AZ Design:** Plan subnets across multiple Availability Zones
- **Network Segmentation:** Consider separate subnets for different tiers (web, app, db)
- **DNS Configuration:** May need to enable DNS hostnames for some applications
- **VPC Peering:** Plan for potential connections to other VPCs
- **Cost Management:** VPC itself is free, but associated resources have costs

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **VPC Fundamentals:** Understanding of AWS Virtual Private Cloud concepts
- **CIDR Block Planning:** IP address range allocation and subnet planning
- **Network Isolation:** Creating isolated network environments in AWS

**Terraform Features Used:**
- `aws_vpc` - VPC resource creation
- `cidr_block` - IP address range configuration
- `tags` - Resource labeling and organization
- Basic resource definition and attributes

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **VPC Name** - Created "nautilus-vpc" as required
- **Region** - Deployed in us-east-1 as specified
- **CIDR Block** - Configured with 10.0.0.0/16 IPv4 range
- **File Structure** - Used single `main.tf` file as required
- **Tagging** - Proper resource naming and identification

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Network Foundation:** The VPC is now ready to host subnets, security groups, and other networking components for the Nautilus application infrastructure migration.

### 🔮 Next Steps
This VPC can now be extended with:
- Public and private subnets
- Internet Gateway for external connectivity
- Route tables for traffic routing
- NAT Gateway for private subnet internet access
- VPC Endpoints for AWS service connectivity
```