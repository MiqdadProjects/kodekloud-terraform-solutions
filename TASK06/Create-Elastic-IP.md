```markdown
# 🌟 Task 6 - Create Elastic IP Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- Allocate an Elastic IP address named **`datacenter-eip`**
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Create an Elastic IP address that can be used for static IP assignments to AWS resources during the infrastructure migration.

💡 **Note:** Elastic IP addresses provide static IPv4 addresses that can be associated with EC2 instances, NAT gateways, and other AWS resources.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (default region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- AWS Elastic IP (EIP) for static IP addressing
- Reserved public IPv4 address for consistent connectivity
- Foundation for attaching to future EC2 instances or load balancers

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **Elastic IP Resource:** Allocates a static public IPv4 address from AWS pool
- **Static IP Assignment:** Provides consistent external IP addressing
- **Resource Tagging:** Proper naming convention for resource identification
- **Migration Readiness:** Reserved IP address available for service attachment

### 🎯 Implementation Strategy
1. Define Elastic IP resource allocation
2. Configure proper tagging with required name
3. Ensure IP is allocated from AWS's pool of public IPv4 addresses
4. Prepare for future association with AWS resources

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

# Allocate Elastic IP for datacenter infrastructure
resource "aws_eip" "datacenter" {
  tags = {
    Name = "datacenter-eip"
  }
}
```

**Configuration Breakdown:**
- `aws_eip` - Allocates an Elastic IP address from AWS
- `tags` - Assigns the required name "datacenter-eip" for identification
- No domain specification defaults to VPC domain (modern default)

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

  # aws_eip.datacenter will be created
  + resource "aws_eip" "datacenter" {
      + allocation_id        = (known after apply)
      + association_id       = (known after apply)
      + carrier_ip           = (known after apply)
      + customer_owned_ip    = (known after apply)
      + domain               = (known after apply)
      + id                   = (known after apply)
      + instance             = (known after apply)
      + network_border_group = (known after apply)
      + network_interface    = (known after apply)
      + private_dns          = (known after apply)
      + private_ip           = (known after apply)
      + public_dns           = (known after apply)
      + public_ip            = (known after apply)
      + public_ipv4_pool     = "amazon"
      + tags                 = {
          + "Name" = "datacenter-eip"
        }
      + tags_all             = {
          + "Name" = "datacenter-eip"
        }
      + vpc                  = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Allocate Elastic IP for datacenter infrastructure
resource "aws_eip" "datacenter" {
  tags = {
    Name = "datacenter-eip"                # Required name tag for EIP identification
  }
}

# Note: Additional optional configurations for reference:
# domain = "vpc"                          # Explicit VPC domain (default for new accounts)
# public_ipv4_pool = "amazon"             # Use Amazon's pool (default)
# network_border_group = "us-east-1"      # Network border group (auto-detected)
```

### Elastic IP Attributes Explained
| Attribute | Description | Value |
|-----------|-------------|-------|
| `allocation_id` | Unique identifier for the EIP | Generated by AWS |
| `public_ip` | The actual public IP address | Assigned from Amazon pool |
| `domain` | Whether EIP is in VPC or EC2-Classic | "vpc" (default for new accounts) |
| `network_border_group` | Network border group location | Auto-detected based on region |
| `public_ipv4_pool` | IP pool source | "amazon" (default) |

### Default Behavior
- **Domain:** Automatically set to "vpc" for new AWS accounts
- **IP Pool:** Uses Amazon's public IPv4 pool by default
- **Region:** Allocated in the same region as the configured AWS provider
- **Availability:** Remains allocated until explicitly released

---

## ✅ Verification Steps

### Step 1: Verify EIP Allocation
```bash
# List Terraform state to confirm resources
terraform state list
```

**Expected Output:**
```
aws_eip.datacenter
```

### Step 2: Check EIP Details
```bash
# Show detailed EIP information
terraform show
```

### Step 3: Get Allocated IP Address
```bash
# Extract the allocated public IP address
terraform state show aws_eip.datacenter | grep public_ip
```

**Expected Output:**
```
public_ip = "X.X.X.X"  # Actual public IP assigned by AWS
```

---

## 🧪 Testing

### Verify EIP Properties
```bash
# Show complete EIP details from Terraform state
terraform state show aws_eip.datacenter
```

### AWS CLI Verification
```bash
# Describe the allocated EIP
aws ec2 describe-addresses --filters "Name=tag:Name,Values=datacenter-eip"
```

**Expected JSON Output:**
```json
{
    "Addresses": [
        {
            "PublicIp": "X.X.X.X",
            "AllocationId": "eipalloc-xxxxxxxxxxxxxxxxx",
            "Domain": "vpc",
            "NetworkBorderGroup": "us-east-1",
            "PublicIpv4Pool": "amazon",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "datacenter-eip"
                }
            ]
        }
    ]
}
```

### Test EIP Availability
```bash
# Check if EIP is available (not associated with any resource)
aws ec2 describe-addresses --allocation-ids $(terraform state show aws_eip.datacenter | grep allocation_id | cut -d'"' -f4)
```

### Network Connectivity Test
```bash
# Test if the IP is reachable (should timeout as nothing is attached)
ping -c 3 $(terraform output -raw eip_address) || echo "EIP allocated but not attached to any resource"
```

---

## 📚 Quick Reference

### Complete Solution
```hcl
# Allocate Elastic IP for datacenter infrastructure
resource "aws_eip" "datacenter" {
  tags = {
    Name = "datacenter-eip"
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### Post-Deployment Commands
```bash
# View allocated EIP details
terraform state show aws_eip.datacenter

# Get the public IP address
terraform state show aws_eip.datacenter | grep "public_ip.*="
```

### Optional: Add Outputs for Easy Reference
```hcl
# Add to main.tf for convenient IP address access
output "eip_id" {
  description = "Allocation ID of the Elastic IP"
  value       = aws_eip.datacenter.id
}

output "eip_address" {
  description = "The Elastic IP address"
  value       = aws_eip.datacenter.public_ip
}

output "eip_allocation_id" {
  description = "The allocation ID of the Elastic IP"
  value       = aws_eip.datacenter.allocation_id
}
```

### Extended Configuration Example
```hcl
# Alternative configuration with explicit settings
resource "aws_eip" "datacenter" {
  domain               = "vpc"          # Explicit VPC domain
  public_ipv4_pool     = "amazon"       # Use Amazon's IPv4 pool
  network_border_group = "us-east-1"    # Explicit network border group
  
  tags = {
    Name        = "datacenter-eip"
    Environment = "production"
    Project     = "nautilus-migration"
    Purpose     = "datacenter-services"
  }
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: EIP allocation limit exceeded**
- **Symptoms:** Error: "AddressLimitExceeded: The maximum number of addresses has been reached"
- **Solution:** Check and manage existing EIPs or request limit increase
```bash
# List all EIPs in the region
aws ec2 describe-addresses --query 'Addresses[*].[PublicIp,AllocationId,Tags[?Key==`Name`].Value|[0]]' --output table

# Default limit is 5 EIPs per region
# Request increase through AWS Support if needed
```

**Issue 2: Insufficient permissions**
- **Symptoms:** Error: "UnauthorizedOperation: You are not authorized to perform this operation"
- **Solution:** Ensure AWS credentials have proper EC2 permissions
```bash
# Required permissions:
# - ec2:AllocateAddress
# - ec2:DescribeAddresses  
# - ec2:CreateTags
```

**Issue 3: EIP not showing immediately**
- **Symptoms:** EIP allocation appears pending
- **Solution:** EIP allocation is usually immediate, check AWS console
```bash
# Verify EIP status
aws ec2 describe-addresses --allocation-ids <allocation-id>
```

**Issue 4: Confusion about EIP costs**
- **Symptoms:** Questions about EIP billing
- **Solution:** Understand EIP pricing model
```bash
# EIP Costs:
# - FREE when associated with a running EC2 instance
# - CHARGED hourly when not associated or instance is stopped
# - CHARGED for additional EIPs beyond the first one per instance
```

---

## 💡 Best Practices Applied

- **🔐 Resource Management:** Proper tagging for identification and cost tracking
- **📊 IP Management:** Reserved static IP for consistent connectivity
- **🏷️ Naming Conventions:** Clear, descriptive naming for resource identification
- **🔄 Migration Strategy:** Allocated IP ready for association with migrated services
- **💰 Cost Awareness:** Understanding EIP billing implications

---

## 🚀 Production Considerations

### Elastic IP Management
- **Association Planning:** Plan which resources will use this EIP
- **DNS Configuration:** Update DNS records to point to the new EIP
- **Security Groups:** Configure security groups for the EIP when attached
- **Monitoring:** Set up monitoring for the IP address usage
- **Documentation:** Document the purpose and assignment of the EIP

### Cost Optimization
```bash
# Cost Management Tips:
├── Associate EIPs with running instances to avoid charges
├── Release unused EIPs to prevent hourly billing
├── Consider using Application Load Balancer instead of multiple EIPs
└── Monitor EIP usage with AWS Cost Explorer
```

### Security Considerations
- **Network ACLs:** Configure appropriate network access control lists
- **Security Groups:** Implement proper firewall rules when EIP is attached
- **SSH Key Management:** Secure access to instances using this EIP
- **Logging:** Enable VPC Flow Logs to monitor traffic to/from the EIP

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Elastic IP Management:** Understanding AWS static IP allocation
- **Network Infrastructure:** Foundation for consistent external connectivity
- **Resource Tagging:** Proper resource identification and management

**Terraform Features Used:**
- `aws_eip` - Elastic IP resource allocation
- `tags` - Resource labeling and organization
- Resource state management for IP addresses

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **EIP Name** - Allocated "datacenter-eip" as required
- **Static IP** - Reserved public IPv4 address from Amazon's pool
- **Resource Management** - Properly tagged for identification
- **File Structure** - Used single `main.tf` file as required
- **Migration Ready** - EIP available for association with datacenter services

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Static IP Foundation:** The Elastic IP is now allocated and ready to be associated with EC2 instances, NAT gateways, or other AWS resources as part of the Nautilus infrastructure migration strategy.

### 🔮 Next Steps for EIP Usage
- **EC2 Association:** Attach to EC2 instances for static IP access
- **NAT Gateway:** Use for consistent outbound internet access
- **Load Balancer:** Associate with Network Load Balancer if needed
- **DNS Records:** Update DNS to point to this static IP
- **Security Configuration:** Configure security groups and NACLs for the IP
```