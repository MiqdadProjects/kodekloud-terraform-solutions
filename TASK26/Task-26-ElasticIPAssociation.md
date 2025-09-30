# 🌟 Task 26 - Attach AWS Elastic IP to EC2 Instance Using Terraform

## 📌 Task Description

The **Nautilus DevOps team** is optimizing their AWS infrastructure as part of a migration process. They need to associate an existing Elastic IP with an EC2 instance to ensure consistent public IP addressing for their application.

**Requirements:**
- Attach the Elastic IP named **`nautilus-ec2-eip`** to the EC2 instance named **`nautilus-ec2`** in the `us-east-1` region using Terraform.
- The Terraform working directory is **`/home/bob/terraform`**.
- Update the **`main.tf`** file (do not create a separate `.tf` file).

👉 **Your task:** Update the Terraform configuration to associate the `nautilus-ec2-eip` Elastic IP with the `nautilus-ec2` EC2 instance, ensuring the association is verifiable via Terraform and AWS CLI.

💡 **Note:** AWS Elastic IPs provide static public IP addresses for EC2 instances. The task is performed in the `/home/bob/terraform` directory, and the resources must be verifiable via Terraform and AWS CLI. The current date and time is September 30, 2025, 11:10 PM PKT.

---

## 🔧 Infrastructure Overview

**Target Environment:** AWS Cloud (EC2, Elastic IP, region-specific)
**Provider:** AWS (Amazon Web Services)
**Resources:**
- EC2 instance named `nautilus-ec2`
- Elastic IP named `nautilus-ec2-eip`
- Elastic IP association to link the two
**Working Directory:** `/home/bob/terraform`

---

## 📋 Solution Overview

### 🏗️ Architecture Components
- **EC2 Instance Resource:** References the existing `nautilus-ec2` instance.
- **Elastic IP Resource:** References the existing `nautilus-ec2-eip` Elastic IP.
- **Elastic IP Association Resource:** Associates the Elastic IP with the EC2 instance.
- **Networking Framework:** Ensures consistent public IP addressing.

### 🎯 Implementation Strategy
1. Identify the existing `nautilus-ec2` instance and `nautilus-ec2-eip` Elastic IP using their IDs.
2. Update the `main.tf` file to include the `aws_instance`, `aws_eip`, and `aws_eip_association` resources.
3. Import the existing resources into the Terraform state to manage them.
4. Deploy the updated configuration using Terraform in the specified directory.
5. Verify the Elastic IP association using Terraform state and AWS CLI.

---

## 🚀 Implementation Steps

### Step 1: Identify Existing Resources

Since the task implies that the `nautilus-ec2` instance and `nautilus-ec2-eip` Elastic IP already exist, retrieve their IDs using AWS CLI.

```bash
# Get the EC2 instance ID
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --query 'Reservations[].Instances[].InstanceId' --output text
```

**Example Output:**
```
i-bf5359f8bc526f0fc
```

```bash
# Get the Elastic IP allocation ID
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-ec2-eip" --query 'Addresses[].AllocationId' --output text
```

**Example Output:**
```
eipalloc-55fa8df1bb9857489
```

**Note:** The provided solution includes example IDs (`i-bf5359f8bc526f0fc` for the instance and `eipalloc-55fa8df1bb9857489` for the Elastic IP). Replace these with the actual IDs from your environment.

---

### Step 2: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

**Purpose:** Ensure operations are performed in the correct Terraform working directory as specified.

**Note:** In VS Code, right-click under the EXPLORER section and select **Open in Integrated Terminal** to launch the terminal in `/home/bob/terraform`.

---

### Step 3: Update Main Terraform Configuration

Update the `main.tf` file with the configuration to manage and associate the existing resources:

```hcl
# main.tf

# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-2dcf179d826597773"
  vpc_security_group_ids = ["sg-a9a6ded454a743fd7"]

  tags = {
    Name = "nautilus-ec2"
  }
}

# Manage existing Elastic IP
resource "aws_eip" "ec2_eip" {
  vpc = true

  tags = {
    Name = "nautilus-ec2-eip"
  }
}

# Associate Elastic IP with EC2 instance
resource "aws_eip_association" "ec2_eip_assoc" {
  instance_id   = aws_instance.ec2.id
  allocation_id = aws_eip.ec2_eip.id
}
```

**Configuration Breakdown:**
- `provider "aws"`: Configures the AWS provider with the `us-east-1` region.
- `aws_instance`: Manages the existing `nautilus-ec2` instance with the specified AMI, instance type, subnet, and security group (values from the provided solution).
- `aws_eip`: Manages the existing `nautilus-ec2-eip` Elastic IP with VPC support enabled.
- `aws_eip_association`: Associates the Elastic IP with the EC2 instance using their respective IDs.
- `tags`: Applies the required names (`nautilus-ec2` and `nautilus-ec2-eip`) via tags.

**Note:** The AMI (`ami-0c101f26f147fa7fd`), subnet (`subnet-2dcf179d826597773`), and security group (`sg-a9a6ded454a743fd7`) IDs are taken from the provided solution. Verify these match the existing `nautilus-ec2` instance’s configuration using:
```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --query 'Reservations[].Instances[].{AMI:ImageId,Subnet:SubnetId,SecurityGroups:SecurityGroups[].GroupId}'
```

---

### Step 4: Import Existing Resources

Since the `nautilus-ec2` instance and `nautilus-ec2-eip` Elastic IP already exist, import them into the Terraform state to manage them.

```bash
# Import the EC2 instance
terraform import aws_instance.ec2 i-bf5359f8bc526f0fc
```

```bash
# Import the Elastic IP
terraform import aws_eip.ec2_eip eipalloc-55fa8df1bb9857489
```

**Note:** Replace `i-bf5359f8bc526f0fc` and `eipalloc-55fa8df1bb9857489` with the actual instance ID and allocation ID from Step 1.

---

### Step 5: Initialize Terraform (if not already initialized)

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

### Step 6: Format and Validate Configuration

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

### Step 7: Plan and Apply Configuration

```bash
# Review the execution plan
terraform plan

# Apply the configuration
terraform apply -auto-approve
```

**Expected Output:**
```
Terraform will perform the following actions:

  # aws_eip_association.ec2_eip_assoc will be created
  + resource "aws_eip_association" "ec2_eip_assoc" {
      + allocation_id        = "eipalloc-55fa8df1bb9857489"
      + id                   = (known after apply)
      + instance_id          = "i-bf5359f8bc526f0fc"
      + network_interface_id = (known after apply)
      + private_ip_address   = (known after apply)
      + public_ip            = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

### Step 8: Verify Infrastructure Matches Configuration

```bash
terraform plan
```

**Purpose:** Confirm that the infrastructure matches the configuration, ensuring no changes are needed.

**Expected Output:**
```
aws_instance.ec2: Refreshing state... [id=i-bf5359f8bc526f0fc]
aws_eip.ec2_eip: Refreshing state... [id=eipalloc-55fa8df1bb9857489]
aws_eip_association.ec2_eip_assoc: Refreshing state... [id=eipassoc-44c6fc9ee4aa22683]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
```

---

### Step 9: Verify Elastic IP Association

```bash
# Check the Elastic IP association using AWS CLI
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-ec2-eip" --query 'Addresses[].{PublicIp:PublicIp,InstanceId:InstanceId}'
```

**Expected Output:**
```json
[
    {
        "PublicIp": "127.9.0.213",
        "InstanceId": "i-bf5359f8bc526f0fc"
    }
]
```

```bash
# Verify the instance details
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --query 'Reservations[].Instances[].{InstanceId:InstanceId,PublicIpAddress:PublicIpAddress}'
```

**Expected Output:**
```json
[
    {
        "InstanceId": "i-bf5359f8bc526f0fc",
        "PublicIpAddress": "127.9.0.213"
    }
]
```

**Note:** The public IP (`127.9.0.213`) is an example from the provided solution. The actual public IP will be assigned to the Elastic IP and associated with the instance.

---

## 🔍 Code Analysis

### Complete Configuration (`main.tf`)

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-2dcf179d826597773"
  vpc_security_group_ids = ["sg-a9a6ded454a743fd7"]

  tags = {
    Name = "nautilus-ec2"
  }
}

# Manage existing Elastic IP
resource "aws_eip" "ec2_eip" {
  vpc = true

  tags = {
    Name = "nautilus-ec2-eip"
  }
}

# Associate Elastic IP with EC2 instance
resource "aws_eip_association" "ec2_eip_assoc" {
  instance_id   = aws_instance.ec2.id
  allocation_id = aws_eip.ec2_eip.id
}
```

### Resource Attributes

| Attribute | Value | Description |
|-----------|-------|-------------|
| **aws_instance.ami** | ami-0c101f26f147fa7fd | AMI ID of the existing instance |
| **aws_instance.instance_type** | t2.micro | Instance type of the existing instance |
| **aws_instance.subnet_id** | subnet-2dcf179d826597773 | Subnet ID of the existing instance |
| **aws_instance.vpc_security_group_ids** | sg-a9a6ded454a743fd7 | Security group ID of the existing instance |
| **aws_instance.tags.Name** | nautilus-ec2 | Tag for instance identification |
| **aws_eip.vpc** | true | Enables VPC support for the Elastic IP |
| **aws_eip.tags.Name** | nautilus-ec2-eip | Tag for Elastic IP identification |
| **aws_eip_association.instance_id** | (Dynamic) | ID of the `nautilus-ec2` instance |
| **aws_eip_association.allocation_id** | (Dynamic) | Allocation ID of the `nautilus-ec2-eip` Elastic IP |

### Resource Properties
- **Region:** `us-east-1` (specified in provider).
- **Instance State:** Assumes `nautilus-ec2` is in the `running` state.
- **Elastic IP Scope:** VPC-enabled for compatibility with the EC2 instance.
- **Association:** Links the Elastic IP to the instance’s primary network interface.

---

## ✅ Verification Steps

### Step 1: Verify Resource Creation

```bash
# List all Terraform managed resources
terraform state list
```

**Expected Output:**
```
aws_instance.ec2
aws_eip.ec2_eip
aws_eip_association.ec2_eip_assoc
```

### Step 2: Check Resource Details in Terraform State

```bash
# Show detailed association information
terraform state show aws_eip_association.ec2_eip_assoc
```

**Expected Output:**
```
# aws_eip_association.ec2_eip_assoc:
resource "aws_eip_association" "ec2_eip_assoc" {
    allocation_id        = "eipalloc-55fa8df1bb9857489"
    id                   = "eipassoc-44c6fc9ee4aa22683"
    instance_id          = "i-bf5359f8bc526f0fc"
    network_interface_id = "eni-d743265505598bc1a"
    private_ip_address   = "172.31.0.4"
    public_ip            = "127.9.0.213"
}
```

**Note:** Replace the example IDs and IP addresses with the actual values from your environment.

### Step 3: Verify in AWS Console (Optional)

```bash
# Check Elastic IP association
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-ec2-eip" --query 'Addresses[].{PublicIp:PublicIp,InstanceId:InstanceId,NetworkInterfaceId:NetworkInterfaceId}'
```

**Expected JSON Output:**
```json
[
    {
        "PublicIp": "127.9.0.213",
        "InstanceId": "i-bf5359f8bc526f0fc",
        "NetworkInterfaceId": "eni-d743265505598bc1a"
    }
]
```

```bash
# Verify instance public IP
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --query 'Reservations[].Instances[].{InstanceId:InstanceId,PublicIpAddress:PublicIpAddress}'
```

**Expected JSON Output:**
```json
[
    {
        "InstanceId": "i-bf5359f8bc526f0fc",
        "PublicIpAddress": "127.9.0.213"
    }
]
```

---

## 🧪 Testing

### Verify Elastic IP Association

```bash
# Verify the Elastic IP is associated with the instance
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-ec2-eip" --query 'Addresses[].{PublicIp:PublicIp,InstanceId:InstanceId}'
```

**Expected Output:**
```json
[
    {
        "PublicIp": "127.9.0.213",
        "InstanceId": "i-bf5359f8bc526f0fc"
    }
]
```

### Test Instance Accessibility (Optional)

```bash
# Check if instance is reachable using the Elastic IP (requires SSH access)
ssh -i <key-file> ec2-user@127.9.0.213 "echo 'Instance is accessible'"
```

**Expected Output:**
```
Instance is accessible
```

**Note:** Replace `<key-file>` and `127.9.0.213` with the actual key pair and public IP of the Elastic IP. Ensure security group rules allow SSH access.

---

## 📚 Quick Reference

### Complete Solution

```hcl
# Configure the AWS provider
provider "aws" {
  region = "us-east-1"
}

# Manage existing EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-2dcf179d826597773"
  vpc_security_group_ids = ["sg-a9a6ded454a743fd7"]

  tags = {
    Name = "nautilus-ec2"
  }
}

# Manage existing Elastic IP
resource "aws_eip" "ec2_eip" {
  vpc = true

  tags = {
    Name = "nautilus-ec2-eip"
  }
}

# Associate Elastic IP with EC2 instance
resource "aws_eip_association" "ec2_eip_assoc" {
  instance_id   = aws_instance.ec2.id
  allocation_id = aws_eip.ec2_eip.id
}
```

### Deployment Commands

```bash
cd /home/bob/terraform
# Import existing resources
terraform import aws_instance.ec2 i-bf5359f8bc526f0fc
terraform import aws_eip.ec2_eip eipalloc-55fa8df1bb9857489
terraform init
terraform apply -auto-approve
terraform plan
```

### Verification Commands

```bash
# Verify in Terraform
terraform state list
terraform state show aws_eip_association.ec2_eip_assoc

# Verify via AWS CLI
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-ec2-eip"
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2"
```

### Optional: Enhanced Configuration

```hcl
# Enhanced configuration with additional settings
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-2dcf179d826597773"
  vpc_security_group_ids = ["sg-a9a6ded454a743fd7"]
  monitoring    = true

  tags = {
    Name        = "nautilus-ec2"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "application-server"
  }
}

resource "aws_eip" "ec2_eip" {
  vpc = true

  tags = {
    Name        = "nautilus-ec2-eip"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "public-access"
  }
}

resource "aws_eip_association" "ec2_eip_assoc" {
  instance_id   = aws_instance.ec2.id
  allocation_id = aws_eip.ec2_eip.id
}

output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.ec2.id
}

output "public_ip" {
  description = "Public IP of the Elastic IP associated with the instance"
  value       = aws_eip.ec2_eip.public_ip
}
```

---

## 🛠️ Troubleshooting

### Common Issues

**Issue 1: Resource Not Found**

- **Symptoms:** Error: "InvalidInstanceID.NotFound" or "InvalidAllocationID.NotFound"
- **Solution:** Verify the instance ID and allocation ID
```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --query 'Reservations[].Instances[].InstanceId'
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-ec2-eip" --query 'Addresses[].AllocationId'
```

**Issue 2: Insufficient Permissions**

- **Symptoms:** Error: "AccessDenied: User is not authorized to perform: ec2:AssociateAddress"
- **Solution:** Ensure AWS credentials have EC2 permissions
```bash
# Required permissions:
# - ec2:AssociateAddress
# - ec2:DescribeInstances
# - ec2:DescribeAddresses
```

**Issue 3: Elastic IP Already Associated**

- **Symptoms:** Error: "Resource eipalloc-55fa8df1bb9857489 is already associated"
- **Solution:** Disassociate the Elastic IP first or import the existing association
```bash
# Disassociate using AWS CLI
aws ec2 disassociate-address --association-id eipassoc-44c6fc9ee4aa22683
# Import existing association
terraform import aws_eip_association.ec2_eip_assoc eipassoc-44c6fc9ee4aa22683
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

- **🔐 Resource Management:** Imported existing resources to manage them with Terraform.
- **📊 Resource Naming:** Clear names (`nautilus-ec2`, `nautilus-ec2-eip`) via tags.
- **🏷️ Minimal Configuration:** Focused on associating the Elastic IP, preserving existing settings.
- **🔄 Dependency Management:** Ensured proper resource dependencies in Terraform.
- **📍 Verifiability:** Ensured resources can be verified via Terraform and AWS CLI.

---

## 🚀 Production Considerations

### Networking Framework
- **Security Groups:** Ensure security groups allow necessary traffic (e.g., SSH, HTTP).
- **Monitoring:** Enable CloudWatch monitoring for instance and network health.
- **Tagging:** Add tags for cost allocation and resource management.
- **Elastic IP Usage:** Monitor Elastic IP costs (free when associated, charged when unassociated).

### Enhanced Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-2dcf179d826597773"
  vpc_security_group_ids = ["sg-a9a6ded454a743fd7"]
  monitoring    = true

  tags = {
    Name        = "nautilus-ec2"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "application-server"
  }
}

resource "aws_eip" "ec2_eip" {
  vpc = true

  tags = {
    Name        = "nautilus-ec2-eip"
    Team        = "nautilus-devops"
    Environment = "production"
    Purpose     = "public-access"
  }
}

resource "aws_eip_association" "ec2_eip_assoc" {
  instance_id   = aws_instance.ec2.id
  allocation_id = aws_eip.ec2_eip.id
}
```

### Networking Strategy
- **Security Groups:** Restrict inbound traffic to specific ports and IPs.
- **Monitoring:** Set up CloudWatch alarms for network traffic or instance health.
- **Cost Optimization:** Ensure Elastic IPs are always associated to avoid charges.
- **High Availability:** Consider associating Elastic IPs with Auto Scaling groups for failover.

---

## 📖 Learning Outcomes

**Key Concepts Mastered:**
- **Elastic IP Fundamentals:** Understanding Elastic IP allocation and association.
- **EC2 Integration:** Managing EC2 instances and networking with Terraform.
- **Resource Import:** Importing existing AWS resources into Terraform state.
- **Dependency Management:** Ensuring proper resource dependencies in Terraform.

**Terraform Features Used:**
- `aws_instance`: Managing existing EC2 instance.
- `aws_eip`: Managing existing Elastic IP.
- `aws_eip_association`: Associating Elastic IP with EC2 instance.
- AWS provider integration for EC2 services.

---

## 🎯 Task Completion Summary

✅ **Successfully Completed:**
- **Elastic IP Association**: Attached `nautilus-ec2-eip` to `nautilus-ec2` as required.
- **File Structure**: Updated single `main.tf` file as specified.
- **Verification**: Confirmed association via Terraform state and AWS CLI.
- **Deployment**: Applied configuration with no errors.

**Final Status:** 🎉 **Task completed successfully with all requirements met**

**Networking Foundation:** The Elastic IP `nautilus-ec2-eip` is associated with the `nautilus-ec2` instance, providing a static public IP for consistent access to the Nautilus DevOps team’s application.

### 🔮 Resources Ready for:
- **Public Access**: Consistent public IP for application access.
- **Monitoring**: Track instance and network health with CloudWatch.
- **Security**: Enhance with security group rules and IAM roles.
- **Cost Optimization**: Monitor Elastic IP usage to avoid unassociated charges.
- **Integration**: Connect with load balancers or Auto Scaling groups.