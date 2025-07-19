# 🌐 Project 3: How to Create a VPC in AWS Console



---
![VPC Architecture](https://github.com/fadykaram88/how-to-create-a-VPC-in-AWS-Console-/blob/main/Final-arc.png?raw=true)

## 🧭 What is a VPC?

A **Virtual Private Cloud (VPC)** is a logically isolated virtual network in the AWS cloud where you can launch AWS resources like EC2 instances.

### 📦 Subnets

Subnets are smaller sections within a VPC. You can:

- Place servers in **Subnet 1**
- Place databases in **Subnet 2**
- Define subnets as **public** (internet-facing) or **private**

---

## 🧱 VPC Components

- **CIDR Block**: Defines the IP range for your VPC.
- **Route Tables**: Define how traffic is routed within your VPC.
- **Internet Gateway**: Connects your VPC to the internet.
- **NAT Gateway**: Allows private subnets to access the internet.
- **Security Groups**: Control traffic at the instance level.
- **Network ACLs**: Control traffic at the subnet level.

---

## 🛠️ Step-by-Step Guide to Creating a VPC

### 🔍 Step 1: Search for "VPC"

- Go to AWS Console → Search "VPC"

### 🧱 Step 2: Create VPC

- Navigate to **Your VPCs** → Click **Create VPC**
- **Name tag**: `Lab VPC`
- **IPv4 CIDR block**: `10.0.0.0/16`
- Click **Create VPC**

### ⚙️ Step 3: Edit DNS Settings

- Go back to **Your VPCs** → Select your VPC
- Click **Actions** → **Edit VPC settings**
- Enable **DNS Hostnames** → Save

💡 *Note: Use Route 53 to assign a custom domain name to your EC2 instead of IP.*

---

## 🌐 Creating Subnets

### 🌐 Step 4: Public Subnet

1. Go to **Subnets** → **Create subnet**
2. VPC: `Lab VPC`
3. Name: `Public Subnet`
4. AZ: `us-east-1a`
5. CIDR: `10.0.0.0/24`
6. Click **Create**
7. Select Public Subnet → **Actions** → **Edit subnet settings**
8. Enable **Auto-assign public IPv4** → Save

### 🔒 Step 5: Private Subnet

1. Go to **Create Subnet**
2. VPC: `Lab VPC`
3. Name: `Private Subnet`
4. AZ: `us-east-1a`
5. CIDR: `10.0.2.0/23`
6. Click **Create**

---

## 🌐 Step 6: Create and Attach Internet Gateway

1. Go to **Internet Gateway** → **Create internet gateway**
2. Name: `Lab IGW` → Click **Create**
3. Select IGW → **Actions** → **Attach to VPC** → Choose `Lab VPC`

---

## 🛣️ Step 7: Configure Route Tables

### 🔁 Route Tables Overview

Each subnet must be associated with a route table to control network traffic.

### 🛣️ Step A: Rename Existing Table

- Go to **Route Tables**
- Find the default one for `Lab VPC`
- Rename to `Private Route Table`

### 🛣️ Step B: Create New Table

- Click **Create Route Table**
- Name: `Public Route Table`
- VPC: `Lab VPC`

### ➕ Step C: Add Route

- Select `Public Route Table` → **Routes** → **Edit Routes**
- Add Route:
  - **Destination**: `0.0.0.0/0`
  - **Target**: Select `Lab IGW`
- Save

### 🔁 Step D: Associate Subnet

- Go to **Subnet Associations** → **Edit**
- Select **Public Subnet** → Save

✅ *Your Public Subnet is now internet-facing!*

---

## 🔐 Step 8: Create Security Group for App Server

1. Go to **Security Groups** → **Create Security Group**
2. Name: `App-SG`, Description: `Allow HTTP traffic`, VPC: `Lab VPC`
3. **Inbound Rules**:
   - Type: `HTTP`
   - Source: `Anywhere-IPv4`
   - Description: `Allow web access`
4. Click **Create Security Group**

---

## 🚀 Step 9: Launch EC2 App Server

1. Go to **EC2** → **Instances** → **Launch Instance**

2. Name: `App Server`

3. AMI: `Amazon Linux 2023 kernel-6.1 AMI`

4. Instance type: `t3.micro`

5. Key pair:

   - Create new key pair: `Vockey` → Type: `.pem`

6. Network settings:

   - VPC: `Lab VPC`
   - Subnet: `Public Subnet`
   - Firewall: Select `App-SG`

7. Expand **Advanced Details** → **User Data**:

```bash
#!/bin/bash
# Install Apache Web Server and PHP
dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
dnf install -y mariadb105-server
# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACACAD-3-113230/06-lab-mod7-guided-VPC/s3/scripts/al2023-inventory-app.zip -O inventory-app.zip
unzip inventory-app.zip -d /var/www/html/
# Install AWS SDK
wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
unzip aws.zip -d /var/www/html
# Start Web Server
systemctl enable httpd
systemctl start httpd
```

8. Click **Launch Instance**
9. Wait for 3/3 status check
10. Copy **Public DNS** and open it → 🎉 App Server is live!

---

## 📝 Final Notes

- ✅ Your VPC is now ready.
- ✅ Public & Private Subnets working.
- ✅ Route tables and IGW configured.
- ✅ EC2 app server launched and accessible.

🚨 **Important**: This VPC will be reused across more than 15 projects!

---

## 👋 See You in the Next Project

Goodbye for now!

---

