# AWS Internship Notes – 3 Weeks (Bootlabs)

This document summarizes everything I learned and implemented during my **3-week AWS internship** at Bootlabs.  
The content is organized in a **logical flow**, starting from cloud fundamentals to real-world deployment, making it easy to recall for interviews and future reference.

---

## Table of Contents

1. [AWS & Cloud Fundamentals](#1-aws--cloud-fundamentals)
2. [Networking – VPC](#2-networking--vpc-very-important)
3. [EC2 & Web Servers](#3-ec2--web-servers)
4. [Storage – EBS vs S3](#4-storage--ebs-vs-s3-important-difference)
5. [IAM – Security & Access Control](#5-iam--security--access-control)
6. [AWS Lambda & S3 Trigger](#6-aws-lambda--s3-trigger)
7. [3-Tier Web Application](#7-3-tier-web-application-final-week--big-task)
8. [Monitoring – CloudWatch](#8-monitoring--cloudwatch)
9. [Interview Summary](#9-interview-summary-important)
10. [Quick Reference](#10-lifetime-memory-anchors)

---

## 1. AWS & Cloud Fundamentals

### What is AWS? 

AWS (Amazon Web Services) is a **cloud platform** that provides on-demand computing resources such as: 

- Virtual servers
- Storage
- Networking
- Security
- Monitoring

Instead of buying physical servers, AWS allows us to **rent resources and scale as needed**.

### Core Cloud Concepts

| Concept | Description |
|---------|-------------|
| **Region** | A geographical location (e.g., Mumbai) |
| **Availability Zone (AZ)** | Isolated data centers within a region |
| **High Availability** | Applications run across multiple AZs |
| **Pay-as-you-go** | Pay only for what you use |

---

## 2. Networking – VPC (Very Important)

### What is a VPC?

A **VPC (Virtual Private Cloud)** is a **private network** in AWS where we launch resources securely.

> 💡 **Think of it as:** A company's internal network in the cloud

### CIDR Block

- Defines the IP address range for the VPC
- **Example:** `10.0.0.0/16`

### Subnets

Subnets divide the VPC into smaller networks. 

#### 🌐 Public Subnet

- Has route to the **Internet Gateway**
- **Used for:**
  - Web servers
  - Bastion hosts
  - Load balancers

#### 🔒 Private Subnet

- No direct internet access
- **Used for:**
  - Databases
  - Backend servers
  - Application servers

> **🎯 Key Concept (Interview Focus):**
> 
> - **Public subnet** → Internet access via Internet Gateway
> - **Private subnet** → No direct internet access

### Internet Gateway (IGW)

- Allows **internet access** to resources in public subnet
- Attached to the VPC
- Enables bidirectional internet traffic

### NAT Gateway

> ⚠️ **Private Subnet Confusion – CLEARED**

- Allows **private subnet instances to access the internet**
- Blocks **incoming internet traffic**

**Why NAT is needed:**

- Private instances need updates (yum, apt, patches)
- But must not be exposed to the internet

```
Private EC2 → NAT Gateway → Internet (OUTBOUND ONLY)
```

### Route Table

Defines **where traffic goes**. 

| Route Table Type | Destination | Target |
|-----------------|-------------|--------|
| **Public** | `0.0.0.0/0` | Internet Gateway |
| **Private** | `0.0.0.0/0` | NAT Gateway |

---

## 3. EC2 & Web Servers

### EC2 Instance

EC2 is a **virtual server** in AWS. 

**You choose:**

- AMI (Operating System)
- Instance type (CPU, RAM)
- Key pair (for SSH access)
- Security group (firewall rules)
- Subnet (network location)

### SSH Connection to EC2

**Steps followed:**

1. Open Ubuntu terminal
2. Set key permissions:

```bash
chmod 400 key1.pem
```

3. Connect to instance:

```bash
ssh -i "key1.pem" ec2-user@<public-ip>
```

### Web Server Deployment

#### NGINX

- Installed and ran NGINX on a public EC2
- Deployed an HTML page

```bash
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

#### Apache

- Installed and ran Apache HTTP server
- Hosted a basic web page

```bash
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

**Purpose:**

- Understand real server deployment
- Practice public access through security groups

---

## 4. Storage – EBS vs S3 (IMPORTANT DIFFERENCE)

### EBS (Elastic Block Store)

- **Block storage**
- Attached to **one EC2 instance** at a time
- Used like a **hard disk**

**What I did:**

1. Created an EBS volume
2. Attached it to EC2
3. Formatted and mounted it

**Use cases:**

- Databases
- OS disks
- Application data

### S3 (Simple Storage Service)

- **Object storage**
- Not attached to EC2
- Accessible via **internet or API**

**What I did:**

1. Created S3 bucket
2. Uploaded files
3. Enabled versioning

**Use cases:**

- Images
- Videos
- Backups
- Logs
- Static website content

### EBS vs S3 (Memory Anchor)

```
EBS → Hard disk for EC2 (block storage)
S3  → Cloud file storage (object storage)
```

| Feature | EBS | S3 |
|---------|-----|-----|
| **Type** | Block Storage | Object Storage |
| **Attachment** | Attached to EC2 | Independent service |
| **Access** | File system | HTTP/API |
| **Use Case** | Databases, OS | Media, backups |

---

## 5. IAM – Security & Access Control

### What is IAM? 

**IAM (Identity and Access Management)** controls: 

- **Who** can access AWS
- **What** actions they can perform

### IAM Role with S3 Access

**What I did:**

1. Created IAM role with S3 full access
2. Attached role to EC2
3. Used AWS CLI from EC2 to upload/download S3 objects

```bash
aws s3 ls
aws s3 cp file.txt s3://my-bucket/
aws s3 sync .  s3://my-bucket/
```

> ✅ **Why roles instead of access keys?**
> 
> - More secure
> - No hard-coded credentials
> - Automatic credential rotation

---

## 6. AWS Lambda & S3 Trigger

### Lambda Function

Lambda is **serverless compute**:

- No server management
- Runs code only when triggered
- Pay only for execution time

### S3 Triggered Lambda (Real Project)

**What I implemented:**

1. **S3 event trigger:** ObjectCreated
2. **Python Lambda** using `boto3`
3. **Resized uploaded images**
4. **Stored resized image** in another S3 folder

**Workflow:**

```
User uploads image to S3 
    ↓
S3 triggers Lambda
    ↓
Lambda resizes image
    ↓
Saves to processed/ folder
```

### Lambda Layer

- Created a Lambda layer for image processing libraries
- Used AWS CLI to create and attach the layer

**Purpose:**

- Keep Lambda function lightweight
- Reuse dependencies across multiple functions

---

## 7. 3-Tier Web Application (Final Week – BIG TASK)

### Architecture

```
┌─────────────────────────────────────────┐
│  Frontend (EC2 - Public Subnet)         │
│  - Web Server (NGINX/Apache)            │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│  Backend (EC2 - Private Subnet)         │
│  - Application Logic                    │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│  Database (Private Subnet)              │
│  - MySQL/PostgreSQL                     │
└─────────────────────────────────────────┘
```

### What I Did

- Followed AWS reference documentation
- Deployed a complete 3-tier architecture
- Understood real-world cloud design
- Practiced security separation

**Key Learning:**

> ⚠️ **Never expose backend or database publicly**
> 
> - Public layer only serves client requests
> - Backend communicates internally
> - Database isolated in private subnet

---

## 8. Monitoring – CloudWatch

### CloudWatch

AWS monitoring service that tracks:

- CPU utilization
- Memory usage
- Disk I/O
- Network traffic
- Application logs
- Custom metrics

**Use cases:**

- Monitor EC2 performance
- Detect issues early
- Set up alarms for critical metrics
- Debug application issues

---

## 9.  Summary 

> *"During my AWS internship at Bootlabs, I worked on: *
> 
> - *VPC networking with public and private subnets*
> - *EC2 deployment and web server hosting (NGINX, Apache)*
> - *NAT Gateway for private subnet internet access*
> - *EBS and S3 storage solutions*
> - *IAM roles for secure access management*
> - *Serverless Lambda functions with S3 triggers for image processing*
> - *Deployed a 3-tier web application following AWS best practices*
> - *CloudWatch monitoring for performance tracking*
> *This gave me hands-on experience with AWS cloud architecture, security, and real-world application deployment."*

---


```
VPC     → Private network in the cloud
Public  → Internet access via IGW
Private → No internet access (secure)
NAT     → Outbound-only internet for private subnets
EC2     → Virtual server
EBS     → Disk for EC2 (block storage)
S3      → Object storage (files, media, backups)
IAM     → Security and access control
Lambda  → Serverless compute
3-tier  → Real production architecture
```

### Quick Reference Table

| Service | Purpose | Key Feature |
|---------|---------|-------------|
| **VPC** | Private network | Isolation and security |
| **EC2** | Virtual server | Compute power |
| **EBS** | Block storage | Attached to EC2 |
| **S3** | Object storage | Scalable file storage |
| **IAM** | Access control | Roles and permissions |
| **Lambda** | Serverless | Event-driven execution |
| **NAT Gateway** | Internet access | Outbound only for private subnets |
| **CloudWatch** | Monitoring | Logs and metrics |

---

## Key Takeaways

✅ Hands-on experience with core AWS services  
✅ Understanding of cloud networking and security  
✅ Real-world application deployment  
✅ Serverless architecture implementation  
✅ Best practices for production environments  

---
