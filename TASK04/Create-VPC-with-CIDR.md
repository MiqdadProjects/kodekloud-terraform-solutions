```markdown
# 🌟 Task 4 - Create VPC with CIDR Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategically planning the migration of a portion of their infrastructure to the **AWS cloud**. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

This strategic approach allows the team to create isolated network environments for different applications and services, providing better security, organization, and resource management throughout the migration process.

**Requirements:**
- Create a VPC named **`datacenter-vpc`**
- Deploy in region **`us-east-1`**
- Use specific IPv4 CIDR block **`192.168.0.0/24`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create a Virtual Private Cloud (VPC) with a specific CIDR block to establish a focused network environment for datacenter services migration.

💡 **Note:** The /24 CIDR block provides a smaller, more controlled network segment suitable for specific application deployments.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (us-east-1 region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- AWS VPC with specific IPv4 CIDR block (192.168.0.0/24)
- Controlled network segment for datacenter services
- Foundation for targeted service deployment

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **VPC Resource:** Creates an isolated virtual network with specific CIDR range
- **IPv4 CIDR Block:** 192.168.0.0/24 provides 254 usable IP addresses
- **Resource Tagging:** Proper naming convention with "datacenter-vpc"
- **Regional Deployment:** Specifically deployed in us-east-1 region

### 🎯 Implementation Strategy
1. Define VPC resource with precise CIDR block requirement
2. Configure proper naming through tags for datacenter identification
3. Use specific private IP range for controlled networking
4. Ensure compliance with exact CIDR specification

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

# Create VPC for datacenter infrastructure with specific CIDR
resource "aws_vpc" "datacenter_vpc" {
  cidr_block = "192.168.0.0/24"
  
  tags = {
    Name = "datacenter-vpc"
  }
}
```

**Configuration Breakdown:**
- `aws_vpc` - Creates a Virtual Private Cloud in AWS
- `cidr_block = "192.168.0.0/24"` - Specific IP address range (254 usable IP addresses)
- `tags` - Assigns the required name "datacenter-vpc" for identification

---

### Step 3: Initialize Terraform (First Time Only)
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

  # aws_vpc.datacenter_vpc will be created
  + resource "aws_vpc" "datacenter_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                          = "192.168.0.0/24"
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
          + "Name" = "datacenter-vpc"
        }
      + tags_all                            = {
          + "Name" = "datacenter-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Create VPC for datacenter infrastructure with specific CIDR
resource "aws_vpc" "datacenter_vpc" {
  cidr_block = "192.168.0.0/24"       # Specific IPv4 CIDR block as required
                                       # Provides 254 usable IP addresses
                                       # Range: 192.168.0.1 - 192.168.0.254
  
  tags = {
    Name = "datacenter-vpc"            # Required name tag for resource identification
  }
}
```

### CIDR Block Analysis
- **192.168.0.0/24:** Private IP range suitable for internal datacenter networking
- **Network Address:** 192.168.0.0 (not usable for hosts)
- **Broadcast Address:** 192.168.0.255 (not usable for hosts)
- **Usable IP Range:** 192.168.0.1 to 192.168.0.254 (254 addresses)
- **Subnet Mask:** 255.255.255.0
- **Network Size:** Smaller, focused network suitable for specific applications

### Comparison with Previous Task
| Aspect | Task 3 (nautilus-vpc) | Task 4 (datacenter-vpc) |
|--------|----------------------|-------------------------|
| CIDR Block | 10.0.0.0/16 | 192.168.0.0/24 |
| Usable IPs | 65,534 | 254 |
| Subnet Capacity | 256 /24 subnets | 1 network segment |
| Use Case | Large-scale migration | Focused datacenter services |

---

## ✅ Verification Steps

### Step 1: Verify VPC Creation
```bash
# List Terraform state to confirm resources
terraform state list
```

**Expected Output:**
```
aws_vpc.datacenter_vpc
```

### Step 2: Check VPC Details
```bash
# Show detailed VPC information
terraform show
```

### Step 3: Verify Specific CIDR Block
```bash
# Check CIDR block specifically
terraform state show aws_vpc.datacenter_vpc | grep cidr_block
```

**Expected Output:**
```
cidr_block = "192.168.0.0/24"
```

---

## 🧪 Testing

### Verify VPC Properties
```bash
# Show complete VPC details from Terraform state
terraform state show aws_vpc.datacenter_vpc
```

### AWS CLI Verification
```bash
# Describe the created VPC with specific CIDR
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=datacenter-vpc" "Name=cidr,Values=192.168.0.0/24" --region us-east-1
```

**Expected JSON Output:**
```json
{
    "Vpcs": [
        {
            "CidrBlock": "192.168.0.0/24",
            "DhcpOptionsId": "dopt-xxxxxxxxx",
            "State": "available",
            "VpcId": "vpc-xxxxxxxxxxxxxxxxx",
            "OwnerId": "xxxxxxxxxxxx",
            "InstanceTenancy": "default",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "datacenter-vpc"
                }
            ]
        }
    ]
}
```

### Network Validation
```bash
# Verify the exact CIDR block assignment
aws ec2 describe-vpcs --vpc-ids $(terraform output -raw vpc_id) --query 'Vpcs[0].CidrBlock' --output text
```

**Expected Output:**
```
192.168.0.0/24
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Create VPC for datacenter infrastructure with specific CIDR
resource "aws_vpc" "datacenter_vpc" {
  cidr_block = "192.168.0.0/24"
  
  tags = {
    Name = "datacenter-vpc"
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init      # Only first time
terraform plan      # Review the execution plan
terraform apply -auto-approve
```

### Post-Deployment Verification
```bash
# Verify deployment
terraform state list
terraform show | grep cidr_block
```

### Optional: Add Output for Reference
```hcl
# Add to main.tf for easy access to VPC details
output "datacenter_vpc_id" {
  description = "ID of the datacenter VPC"
  value       = aws_vpc.datacenter_vpc.id
}

output "datacenter_vpc_cidr" {
  description = "CIDR block of the datacenter VPC"
  value       = aws_vpc.datacenter_vpc.cidr_block
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: CIDR block format validation**
- **Symptoms:** Error: "Invalid CIDR block format"
- **Solution:** Ensure exact format "192.168.0.0/24"
```bash
# Verify CIDR format before applying
terraform validate
```

**Issue 2: CIDR block conflicts with existing VPCs**
- **Symptoms:** Error: "InvalidVpc.Range: The CIDR conflicts with another VPC"
- **Solution:** Check existing VPCs for overlapping ranges
```bash
# List all VPCs and their CIDR blocks
aws ec2 describe-vpcs --query 'Vpcs[*].[VpcId,CidrBlock,State,Tags[?Key==`Name`].Value|[0]]' --output table
```

**Issue 3: Insufficient IP addresses for requirements**
- **Symptoms:** Planning shows need for more IPs than /24 provides
- **Solution:** /24 provides only 254 usable IPs, plan accordingly
```bash
# Calculate IP requirements:
# /24 = 254 usable IPs
# /23 = 510 usable IPs  
# /22 = 1022 usable IPs
```

**Issue 4: Private IP range concerns**
- **Symptoms:** Questions about 192.168.x.x range usage
- **Solution:** 192.168.0.0/16 is reserved for private use (RFC 1918)
```bash
# Private IP ranges:
# 10.0.0.0/8        (10.0.0.0 - 10.255.255.255)
# 172.16.0.0/12     (172.16.0.0 - 172.31.255.255)  
# 192.168.0.0/16    (192.168.0.0 - 192.168.255.255)
```

---

## 💡 Best Practices Applied

- **🔐 Network Security:** Using private IP range (192.168.x.x) for internal communication
- **📊 Resource Management:** Specific CIDR block for controlled network sizing
- **🏷️ Naming Conventions:** Clear, descriptive naming matching datacenter context
- **🔄 CIDR Planning:** Precise /24 allocation for focused application deployment
- **📍 Regional Strategy:** Explicit deployment in us-east-1 region

---

## 🚀 Production Considerations

- **Scalability:** /24 CIDR provides 254 IPs - suitable for smaller deployments
- **Network Design:** Consider if more IP addresses will be needed for growth
- **Subnet Strategy:** Single /24 can be used as one subnet or remain unsegmented
- **Integration:** Plan connectivity with other VPCs if needed
- **Monitoring:** Consider VPC Flow Logs for network traffic analysis
- **Security:** Implement proper security groups and NACLs

### Network Planning Guidelines
```
192.168.0.0/24 - Datacenter VPC
├── 192.168.0.1-50    - Web tier (if needed)
├── 192.168.0.51-100  - Application tier (if needed)
├── 192.168.0.101-150 - Database tier (if needed)
└── 192.168.0.151-254 - Management/utility (if needed)
```

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Specific CIDR Planning:** Working with precise network requirements
- **Network Sizing:** Understanding /24 network limitations and benefits
- **VPC Design Patterns:** Creating focused, purpose-built network segments

**Terraform Features Used:**
- `aws_vpc` - VPC resource with specific CIDR requirements
- `cidr_block` - Precise IP address range configuration
- `tags` - Resource identification and management
- Exact requirement compliance

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **VPC Name** - Created "datacenter-vpc" as required
- **Region** - Deployed in us-east-1 as specified
- **CIDR Block** - Configured with exact 192.168.0.0/24 range
- **File Structure** - Used single `main.tf` file as required
- **IP Allocation** - Provided 254 usable IP addresses for datacenter services

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Network Foundation:** The datacenter VPC is now ready with a focused network segment that provides controlled IP allocation suitable for specific datacenter service migrations.

### 🔮 Network Capacity Summary
- **Total Network:** 192.168.0.0/24
- **Usable Range:** 192.168.0.1 - 192.168.0.254
- **Capacity:** 254 IP addresses
- **Use Case:** Focused datacenter services deployment
- **Future Expansion:** Consider additional VPCs for scaling beyond 254 IPs
```