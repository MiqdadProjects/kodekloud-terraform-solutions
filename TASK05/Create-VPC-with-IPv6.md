```markdown
# 🌟 Task 5 - Create VPC with IPv6 Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategically planning the migration of a portion of their infrastructure to the **AWS cloud**. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

This task focuses on creating a modern, dual-stack VPC that supports both IPv4 and IPv6 networking, preparing the infrastructure for future-ready applications that can leverage the expanded address space and enhanced features of IPv6 protocol.

**Requirements:**
- Create a VPC named **`nautilus-vpc`**
- Deploy in region **`us-east-1`**
- Use Amazon-provided IPv6 CIDR block
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create a dual-stack VPC with both IPv4 and Amazon-provided IPv6 CIDR blocks to support modern networking requirements.

💡 **Note:** Amazon automatically assigns a /56 IPv6 CIDR block from Amazon's pool of IPv6 addresses when enabled.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- AWS VPC with dual-stack networking (IPv4 + IPv6)
- IPv4 CIDR block for backward compatibility
- Amazon-provided IPv6 CIDR block for modern applications
- Future-ready network infrastructure

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **VPC Resource:** Creates a dual-stack virtual network supporting both IP versions
- **IPv4 CIDR Block:** Standard private IP range (10.0.0.0/16) for existing services
- **IPv6 CIDR Block:** Amazon-provided /56 block for global IPv6 connectivity
- **Dual-Stack Support:** Enables both IPv4 and IPv6 communication protocols
- **Resource Tagging:** Proper naming convention for resource identification

### 🎯 Implementation Strategy
1. Define VPC resource with IPv4 CIDR block for compatibility
2. Enable Amazon-provided IPv6 CIDR block assignment
3. Configure proper tagging for resource management
4. Ensure dual-stack networking capability is established

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

# Create VPC with dual-stack networking (IPv4 + IPv6)
resource "aws_vpc" "nautilus" {
  cidr_block                       = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true
  
  tags = {
    Name = "nautilus-vpc"
  }
}
```

**Configuration Breakdown:**
- `aws_vpc` - Creates a Virtual Private Cloud in AWS
- `cidr_block = "10.0.0.0/16"` - IPv4 CIDR block (65,534 usable IP addresses)
- `assign_generated_ipv6_cidr_block = true` - Enables Amazon-provided IPv6 CIDR block
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

  # aws_vpc.nautilus will be created
  + resource "aws_vpc" "nautilus" {
      + arn                                  = (known after apply)
      + assign_generated_ipv6_cidr_block     = true
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
      + ipv6_netmask                        = (known after apply)
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
# Create VPC with dual-stack networking (IPv4 + IPv6)
resource "aws_vpc" "nautilus" {
  cidr_block                       = "10.0.0.0/16"    # IPv4 CIDR block
                                                       # 65,534 usable IPv4 addresses
  
  assign_generated_ipv6_cidr_block = true             # Enable Amazon-provided IPv6
                                                       # AWS assigns a /56 IPv6 CIDR block
  
  tags = {
    Name = "nautilus-vpc"                             # Required VPC name
  }
}
```

### IPv6 CIDR Block Details
- **Amazon Assignment:** AWS automatically provides a /56 IPv6 CIDR block
- **IPv6 Range:** Globally routable IPv6 addresses from Amazon's pool
- **Block Size:** /56 allows for 256 /64 subnets
- **Global Connectivity:** IPv6 addresses are internet-routable by default
- **Format Example:** 2600:1f14:abc:d000::/56 (actual values assigned by AWS)

### Dual-Stack Networking Benefits
| Feature | IPv4 (10.0.0.0/16) | IPv6 (Amazon /56) |
|---------|-------------------|-------------------|
| Address Space | 65,534 addresses | Virtually unlimited |
| Connectivity | Private (NAT required) | Global (direct internet) |
| Subnet Capacity | 256 /24 subnets | 256 /64 subnets |
| Use Case | Legacy compatibility | Modern applications |

---

## ✅ Verification Steps

### Step 1: Verify VPC Creation
```bash
# List Terraform state to confirm resources
terraform state list
```

**Expected Output:**
```
aws_vpc.nautilus
```

### Step 2: Check Both IPv4 and IPv6 CIDR Blocks
```bash
# Show detailed VPC information including IPv6
terraform show
```

### Step 3: Verify IPv6 CIDR Assignment
```bash
# Check specifically for IPv6 CIDR block
terraform state show aws_vpc.nautilus | grep ipv6_cidr_block
```

**Expected Output:**
```
ipv6_cidr_block = "2600:1f14:abc:d000::/56"  # Example format
```

---

## 🧪 Testing

### Verify Dual-Stack Configuration
```bash
# Show complete VPC details from Terraform state
terraform state show aws_vpc.nautilus
```

### AWS CLI Verification
```bash
# Describe the VPC with both CIDR blocks
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=nautilus-vpc" --region us-east-1
```

**Expected JSON Output:**
```json
{
    "Vpcs": [
        {
            "CidrBlock": "10.0.0.0/16",
            "State": "available",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "Ipv6CidrBlockAssociationSet": [
                {
                    "AssociationId": "vpc-cidr-assoc-xxxxxxxxx",
                    "Ipv6CidrBlock": "2600:1f14:abc:d000::/56",
                    "Ipv6CidrBlockState": {
                        "State": "associated"
                    }
                }
            ],
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

### Test IPv6 Assignment
```bash
# Get IPv6 CIDR block specifically
aws ec2 describe-vpcs --vpc-ids $(terraform output -raw vpc_id) --query 'Vpcs[0].Ipv6CidrBlockAssociationSet[0].Ipv6CidrBlock' --output text
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create VPC with dual-stack networking (IPv4 + IPv6)
resource "aws_vpc" "nautilus" {
  cidr_block                       = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true
  
  tags = {
    Name = "nautilus-vpc"
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### Post-Deployment Verification
```bash
# Verify both CIDR blocks
terraform show | grep cidr_block
terraform show | grep ipv6
```

### Optional: Add Outputs for Reference
```hcl
# Add to main.tf for easy access to both CIDR blocks
output "vpc_id" {
  description = "ID of the nautilus VPC"
  value       = aws_vpc.nautilus.id
}

output "ipv4_cidr_block" {
  description = "IPv4 CIDR block of the VPC"
  value       = aws_vpc.nautilus.cidr_block
}

output "ipv6_cidr_block" {
  description = "IPv6 CIDR block of the VPC"
  value       = aws_vpc.nautilus.ipv6_cidr_block
}

output "ipv6_association_id" {
  description = "Association ID for the IPv6 CIDR block"
  value       = aws_vpc.nautilus.ipv6_association_id
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: IPv6 not supported in region**
- **Symptoms:** Error: "IPv6 is not supported in this region"
- **Solution:** Verify us-east-1 supports IPv6 (it does)
```bash
# Check IPv6 availability in region
aws ec2 describe-availability-zones --region us-east-1 --query 'AvailabilityZones[0].RegionName'
```

**Issue 2: IPv6 CIDR block assignment fails**
- **Symptoms:** Error during IPv6 CIDR block association
- **Solution:** Ensure proper IAM permissions and retry
```bash
# Required permissions:
# - ec2:AssociateVpcCidrBlock
# - ec2:DescribeVpcs
# - ec2:CreateVpc
```

**Issue 3: IPv6 CIDR block not showing immediately**
- **Symptoms:** IPv6 CIDR block appears as "pending" or not visible
- **Solution:** Wait for AWS to complete the association (usually < 1 minute)
```bash
# Check association state
aws ec2 describe-vpcs --vpc-ids <vpc-id> --query 'Vpcs[0].Ipv6CidrBlockAssociationSet[0].Ipv6CidrBlockState.State'
```

**Issue 4: Confusion about IPv6 address format**
- **Symptoms:** Unexpected IPv6 CIDR format
- **Solution:** AWS provides addresses in standard IPv6 format
```bash
# IPv6 format examples:
# 2600:1f14:abc:d000::/56  (Amazon's format)
# Each /56 can be divided into 256 /64 subnets
# /64 subnets are standard for IPv6 networks
```

---

## 💡 Best Practices Applied

- **🔐 Dual-Stack Security:** Both IPv4 and IPv6 networking capabilities
- **📊 Resource Management:** Comprehensive tagging for resource identification
- **🏷️ Naming Conventions:** Clear, descriptive naming matching requirements
- **🔄 Future-Ready Design:** IPv6 support for modern application requirements
- **📍 Regional Strategy:** Deployed in us-east-1 with full IPv6 support

---

## 🚀 Production Considerations

- **IPv6 Routing:** Configure route tables for IPv6 traffic
- **Security Groups:** Create rules for both IPv4 and IPv6 traffic
- **Subnets:** Plan /64 IPv6 subnets for different tiers
- **Internet Gateway:** Ensure IPv6 routing to internet when needed
- **DNS:** Configure AAAA records for IPv6-enabled services
- **Monitoring:** Monitor both IPv4 and IPv6 traffic patterns

### IPv6 Subnet Planning
```
Amazon Provided: 2600:1f14:abc:d000::/56
├── 2600:1f14:abc:d000::/64 - Web tier subnet
├── 2600:1f14:abc:d001::/64 - App tier subnet  
├── 2600:1f14:abc:d002::/64 - Database tier subnet
└── 2600:1f14:abc:d003::/64 - Management subnet
... (up to 256 /64 subnets available)
```

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Dual-Stack Networking:** Understanding IPv4 and IPv6 coexistence
- **IPv6 CIDR Management:** Working with Amazon-provided IPv6 blocks
- **Modern VPC Design:** Creating future-ready network infrastructure

**Terraform Features Used:**
- `aws_vpc` - VPC resource with dual-stack capabilities
- `cidr_block` - IPv4 address range configuration
- `assign_generated_ipv6_cidr_block` - Amazon IPv6 CIDR assignment
- Resource attributes for modern networking

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **VPC Name** - Created "nautilus-vpc" as required
- **Region** - Deployed in us-east-1 as specified
- **IPv4 Support** - Configured with 10.0.0.0/16 CIDR block
- **IPv6 Support** - Enabled Amazon-provided IPv6 CIDR block
- **Dual-Stack** - Both IP versions available for services
- **File Structure** - Used single `main.tf` file as required

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Modern Infrastructure:** The VPC now supports both IPv4 and IPv6 networking, providing a future-ready foundation for modern applications that can leverage the expanded address space and global connectivity of IPv6.

### 🔮 Network Capabilities Summary
- **IPv4 Capacity:** 65,534 addresses (10.0.0.0/16)
- **IPv6 Capacity:** Virtually unlimited (/56 from Amazon)
- **Subnet Options:** 256 IPv4 /24 subnets + 256 IPv6 /64 subnets
- **Connectivity:** Private IPv4 + Global IPv6 capabilities
- **Use Cases:** Legacy and modern application support
```