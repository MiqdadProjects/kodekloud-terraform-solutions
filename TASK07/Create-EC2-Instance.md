```markdown
# 🌟 Task 7 - Create EC2 Instance Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

This task focuses on deploying the core compute infrastructure by creating an EC2 instance that will serve as the foundation for running applications and services in the cloud environment.

**Requirements:**
- The name of the instance must be **`xfusion-ec2`**
- Use the Amazon Linux AMI **`ami-0c101f26f147fa7fd`**
- The instance type must be **`t2.micro`**
- Create a new RSA key named **`xfusion-kp`**
- Attach the default (available by default) security group
- The Terraform working directory is **`/home/bob/terraform`**
- Create the **`main.tf`** file (do not create a different .tf file)

👉 **Your task:** Deploy a complete EC2 instance with key pair authentication and proper security configuration for the Nautilus infrastructure migration.

💡 **Note:** This EC2 instance will serve as a foundational compute resource for hosting applications during the migration process.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (default region)
**Provider:** AWS (Amazon Web Services)
**Resources:** 
- EC2 Instance (t2.micro) running Amazon Linux
- RSA Key Pair for secure SSH access
- Default Security Group for network access control
- Local private key file for authentication

**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **TLS Private Key Resource:** Generates RSA private key locally for secure authentication
- **AWS Key Pair Resource:** Registers public key with AWS for EC2 access
- **Local File Resource:** Saves private key locally with secure permissions
- **Data Source:** Retrieves default security group information
- **EC2 Instance Resource:** Provisions virtual server with specified configuration

### 🎯 Implementation Strategy
1. Generate RSA key pair for secure instance access
2. Save private key locally with appropriate permissions
3. Query default security group using data source
4. Launch EC2 instance with specified AMI and configuration
5. Associate key pair and security group with instance

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

# Use a data source to get the default security group
data "aws_security_group" "default" {
  name = "default"
}

# Provision the EC2 instance
resource "aws_instance" "xfusion-ec2" {
  # The AMI ID for Amazon Linux as specified
  ami           = "ami-0c101f26f147fa7fd"
  # The instance type as specified
  instance_type = "t2.micro"
  # Attach the newly created key pair
  key_name      = aws_key_pair.xfusion_kp.key_name
  # Attach the default security group
  vpc_security_group_ids = [data.aws_security_group.default.id]
  
  # Tag the instance with the required name
  tags = {
    Name = "xfusion-ec2"
  }
}
```

**Configuration Breakdown:**
- `tls_private_key` - Generates 4096-bit RSA private key
- `aws_key_pair` - Creates AWS key pair with generated public key
- `local_file` - Saves private key with secure 0600 permissions
- `data "aws_security_group"` - Retrieves default security group
- `aws_instance` - Provisions EC2 instance with specified configuration

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
- Finding latest version of hashicorp/aws...
- Finding latest version of hashicorp/tls...
- Finding latest version of hashicorp/local...
- Installing hashicorp/aws v5.20.0...
- Installing hashicorp/tls v4.0.4...
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

  # aws_instance.xfusion-ec2 will be created
  + resource "aws_instance" "xfusion-ec2" {
      + ami                                  = "ami-0c101f26f147fa7fd"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = "xfusion-kp"
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns_name                     = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns_name                      = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "xfusion-ec2"
        }
      + tags_all                             = {
          + "Name" = "xfusion-ec2"
        }
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)
    }

  # aws_key_pair.xfusion_kp will be created
  + resource "aws_key_pair" "xfusion_kp" {
      + arn         = (known after apply)
      + fingerprint = (known after apply)
      + id          = (known after apply)
      + key_name    = "xfusion-kp"
      + key_pair_id = (known after apply)
      + key_type    = (known after apply)
      + public_key  = (known after apply)
      + tags_all    = (known after apply)
    }

  # local_file.private_key_pem will be created
  + resource "local_file" "private_key_pem" {
      + content              = (sensitive value)
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0777"
      + file_permission      = "0600"
      + filename             = "/home/bob/xfusion-kp.pem"
      + id                   = (known after apply)
    }

  # tls_private_key.xfusion_key will be created
  + resource "tls_private_key" "xfusion_key" {
      + algorithm                     = "RSA"
      + ecdsa_curve                   = "P224"
      + id                            = (known after apply)
      + private_key_openssh           = (sensitive value)
      + private_key_pem               = (sensitive value)
      + private_key_pem_pkcs8         = (sensitive value)
      + public_key_fingerprint_md5    = (known after apply)
      + public_key_fingerprint_sha256 = (known after apply)
      + public_key_openssh            = (known after apply)
      + public_key_pem                = (known after apply)
      + rsa_bits                      = 4096
    }

Plan: 4 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)
```hcl
# Generate RSA private key locally
resource "tls_private_key" "xfusion_key" {
  algorithm = "RSA"                    # RSA algorithm for key generation
  rsa_bits  = 4096                     # 4096-bit key for enhanced security
}

# Create AWS key pair using the public key
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"                                   # Required key pair name
  public_key = tls_private_key.xfusion_key.public_key_openssh # Reference to generated public key
}

# Save private key locally with secure permissions
resource "local_file" "private_key_pem" {
  content         = tls_private_key.xfusion_key.private_key_pem # Private key in PEM format
  filename        = "/home/bob/xfusion-kp.pem"                  # Required file path
  file_permission = "0600"                                      # Secure permissions (read/write owner only)
}

# Use data source to get the default security group
data "aws_security_group" "default" {
  name = "default"                     # Query default security group by name
}

# Provision the EC2 instance
resource "aws_instance" "xfusion-ec2" {
  ami           = "ami-0c101f26f147fa7fd"                    # Specified Amazon Linux AMI
  instance_type = "t2.micro"                                # Required instance type (Free Tier eligible)
  key_name      = aws_key_pair.xfusion_kp.key_name         # Attach created key pair
  vpc_security_group_ids = [data.aws_security_group.default.id] # Attach default security group
  
  tags = {
    Name = "xfusion-ec2"               # Required instance name
  }
}
```

### Resource Dependencies Flow
```
tls_private_key.xfusion_key
    ├── aws_key_pair.xfusion_kp (depends on public key)
    └── local_file.private_key_pem (depends on private key)

data.aws_security_group.default (independent query)

aws_instance.xfusion-ec2
    ├── depends on aws_key_pair.xfusion_kp.key_name
    └── depends on data.aws_security_group.default.id
```

### Instance Specifications
| Component | Value | Description |
|-----------|--------|-------------|
| **AMI** | ami-0c101f26f147fa7fd | Amazon Linux (specific version) |
| **Instance Type** | t2.micro | 1 vCPU, 1 GB RAM (Free Tier) |
| **Key Pair** | xfusion-kp | RSA 4096-bit for SSH access |
| **Security Group** | default | Default VPC security group |
| **Storage** | Default EBS | 8 GB gp2 root volume (default) |

---

## ✅ Verification Steps

### Step 1: Verify All Resources Created
```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_instance.xfusion-ec2
aws_key_pair.xfusion_kp
data.aws_security_group.default
local_file.private_key_pem
tls_private_key.xfusion_key
```

### Step 2: Check Instance Details
```bash
# Show detailed instance information
terraform state show aws_instance.xfusion-ec2
```

### Step 3: Verify Private Key File
```bash
# Check private key file was created with correct permissions
ls -l /home/bob/xfusion-kp.pem
```

**Expected Output:**
```
-rw------- 1 bob bob 3247 [date] /home/bob/xfusion-kp.pem
```

### Step 4: Get Instance Public IP
```bash
# Extract public IP address for SSH access
terraform state show aws_instance.xfusion-ec2 | grep "public_ip.*="
```

---

## 🧪 Testing

### Test SSH Connection to Instance
```bash
# SSH to the EC2 instance using the generated key
ssh -i /home/bob/xfusion-kp.pem ec2-user@$(terraform state show aws_instance.xfusion-ec2 | grep "public_ip " | cut -d'"' -f4)
```

### Verify Instance Status
```bash
# Check instance status using AWS CLI
aws ec2 describe-instances --filters "Name=tag:Name,Values=xfusion-ec2" --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]' --output table
```

### Test Key Pair Validity
```bash
# Verify private key format
openssl rsa -in /home/bob/xfusion-kp.pem -check -noout
```

**Expected Output:**
```
RSA key ok
```

### Verify Security Group Assignment
```bash
# Check assigned security groups
aws ec2 describe-instances --instance-ids $(terraform state show aws_instance.xfusion-ec2 | grep "^id " | cut -d'"' -f4) --query 'Reservations[*].Instances[*].SecurityGroups[*].[GroupName,GroupId]' --output table
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

# Use a data source to get the default security group
data "aws_security_group" "default" {
  name = "default"
}

# Provision the EC2 instance
resource "aws_instance" "xfusion-ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  key_name      = aws_key_pair.xfusion_kp.key_name
  vpc_security_group_ids = [data.aws_security_group.default.id]
  
  tags = {
    Name = "xfusion-ec2"
  }
}
```

### Deployment Commands
```bash
cd /home/bob/terraform
terraform init
terraform apply -auto-approve
```

### SSH Connection
```bash
# Connect to the instance
ssh -i /home/bob/xfusion-kp.pem ec2-user@<public_ip_address>
```

### Optional: Add Outputs for Easy Access
```hcl
# Add to main.tf for convenient access to instance details
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.xfusion-ec2.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.xfusion-ec2.public_ip
}

output "instance_private_ip" {
  description = "Private IP address of the EC2 instance"
  value       = aws_instance.xfusion-ec2.private_ip
}

output "key_pair_name" {
  description = "Name of the key pair"
  value       = aws_key_pair.xfusion_kp.key_name
}

output "ssh_connection_command" {
  description = "SSH command to connect to the instance"
  value       = "ssh -i /home/bob/xfusion-kp.pem ec2-user@${aws_instance.xfusion-ec2.public_ip}"
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: AMI not found**
- **Symptoms:** Error: "InvalidAMIID.NotFound"
- **Solution:** Verify AMI exists in the current region
```bash
# Check if AMI exists in current region
aws ec2 describe-images --image-ids ami-0c101f26f147fa7fd
```

**Issue 2: Instance fails to launch**
- **Symptoms:** Instance goes to "terminated" state
- **Solution:** Check instance limits and subnet capacity
```bash
# Check running instances count
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query 'length(Reservations[*].Instances[*])'

# Check if in correct subnet/AZ
aws ec2 describe-availability-zones --region us-east-1
```

**Issue 3: Cannot SSH to instance**
- **Symptoms:** SSH connection timeout or refused
- **Solution:** Check security group rules and instance status
```bash
# Verify default security group allows SSH (port 22)
aws ec2 describe-security-groups --group-names default --query 'SecurityGroups[*].IpPermissions[?FromPort==`22`]'

# Check instance status
aws ec2 describe-instance-status --instance-ids <instance-id>
```

**Issue 4: Private key permissions error**
- **Symptoms:** SSH rejects private key due to permissions
- **Solution:** Ensure correct file permissions
```bash
# Fix private key permissions
chmod 600 /home/bob/xfusion-kp.pem

# Verify permissions
ls -l /home/bob/xfusion-kp.pem
```

**Issue 5: Default security group not found**
- **Symptoms:** Error: "InvalidGroup.NotFound"
- **Solution:** Ensure default VPC exists or specify VPC ID
```bash
# Check if default VPC exists
aws ec2 describe-vpcs --filters "Name=is-default,Values=true"

# Alternative: Query default security group by VPC
data "aws_security_group" "default" {
  name   = "default"
  vpc_id = data.aws_vpc.default.id
}
```

---

## 💡 Best Practices Applied

- **🔐 Security:** RSA 4096-bit key for enhanced security
- **📊 Resource Management:** Proper dependency management between resources
- **🏷️ Naming Conventions:** Clear, descriptive resource naming
- **🔄 State Management:** All resources managed in Terraform state
- **💰 Cost Optimization:** Using t2.micro (Free Tier eligible) instance type

---

## 🚀 Production Considerations

### Security Enhancements
- **Custom Security Groups:** Create specific security groups instead of using default
- **Key Rotation:** Implement regular key pair rotation strategy
- **Instance Profile:** Attach IAM roles instead of using access keys
- **Private Subnets:** Consider deploying in private subnets with bastion hosts

### Monitoring & Maintenance
- **CloudWatch:** Enable detailed monitoring for the instance
- **Patch Management:** Implement automated patching with Systems Manager
- **Backup Strategy:** Configure automated EBS snapshots
- **Log Collection:** Set up CloudWatch Logs agent

### High Availability
```hcl
# Production considerations for HA:
├── Multiple Availability Zones
├── Auto Scaling Groups
├── Application Load Balancer
├── RDS for database instead of local storage
└── ElastiCache for session management
```

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **EC2 Instance Management:** Complete instance provisioning with Terraform
- **Key Pair Management:** RSA key generation and AWS integration
- **Security Group Integration:** Using data sources for existing resources
- **Resource Dependencies:** Understanding Terraform dependency management

**Terraform Features Used:**
- `aws_instance` - EC2 instance provisioning
- `aws_key_pair` - SSH key management
- `tls_private_key` - Cryptographic key generation
- `local_file` - Local file management
- `data` sources - Querying existing AWS resources

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Instance Name** - Created "xfusion-ec2" as required
- **AMI** - Used specified Amazon Linux AMI (ami-0c101f26f147fa7fd)
- **Instance Type** - Configured t2.micro as specified
- **Key Pair** - Created RSA key "xfusion-kp" with 4096-bit encryption
- **Security Group** - Attached default security group as required
- **File Structure** - Used single `main.tf` file as specified

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Compute Foundation:** The EC2 instance is now running with secure SSH access, ready to host applications and services as part of the Nautilus infrastructure migration to AWS cloud.

### 🔮 Instance Ready for:
- **Application Deployment:** Web servers, databases, or custom applications
- **Development Environment:** Development and testing workloads
- **Service Migration:** Migrating existing services from on-premises
- **Auto Scaling:** Foundation for creating launch templates and auto scaling
- **Load Balancing:** Target for Application or Network Load Balancers
```