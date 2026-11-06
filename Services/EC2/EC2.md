# Amazon EC2 (Elastic Compute Cloud)

## Table of Contents

1. [What is Amazon EC2?](#1-what-is-amazon-ec2)
2. [Why Do We Use EC2?](#2-why-do-we-use-ec2)
3. [Features of EC2](#3-features-of-ec2)
4. [How EC2 Works](#4-how-ec2-works)
5. [EC2 Instance Types](#5-ec2-instance-types)
6. [Important EC2 Components](#6-important-ec2-components)
7. [How to Launch an EC2 Instance](#7-how-to-launch-an-ec2-instance)
8. [How to Connect to an EC2 Instance](#8-how-to-connect-to-an-ec2-instance)
9. [EC2 Instance Lifecycle](#9-ec2-instance-lifecycle)
10. [EC2 Pricing](#10-ec2-pricing)
11. [Practical EC2 Commands Using AWS CLI](#11-practical-ec2-commands-using-aws-cli)
12. [Real-World Use Cases](#12-real-world-use-cases)
13. [EC2 Best Practices](#13-ec2-best-practices)
14. [Common EC2 Errors](#14-common-ec2-errors)
15. [Interview Questions](#15-interview-questions)
16. [Quick Revision](#16-quick-revision)

---

## 1. What is Amazon EC2?

Amazon EC2 stands for **Amazon Elastic Compute Cloud**.

It is an AWS service that provides virtual servers in the cloud. These virtual servers are called EC2 instances.

We can use EC2 instances to run applications, host websites, process data, train machine learning models, and perform other computing tasks.

Instead of purchasing and maintaining physical servers, we can rent computing resources from AWS.

**Example:**

Suppose you want to deploy a Python application.

Instead of purchasing a physical computer and keeping it running 24/7, you can launch an EC2 instance and deploy your application on it.

### Key Points

- EC2 provides virtual machines in the cloud.
- You can choose CPU, RAM, storage, and operating system.
- You can start, stop, reboot, and terminate instances.
- You pay according to the pricing model and resources you use.
- EC2 is an Infrastructure as a Service (IaaS).

## 2. Why Do We Use EC2?

EC2 is used when we need computing power without purchasing physical hardware.

### Main Reasons

1. **Scalability:** Increase or decrease computing resources according to requirements.
2. **Flexibility:** Choose different operating systems, instance types, and configurations.
3. **Cost Efficiency:** Pay according to the selected pricing model and usage.
4. **High Availability:** Deploy instances across multiple Availability Zones.
5. **Security:** Control access using security groups, IAM roles, and network configurations.
6. **Easy Management:** Launch and manage virtual servers through the AWS Console, CLI, and SDKs.

## 3. Features of EC2

| Feature | Description |
|---|---|
| Virtual Servers | Provides cloud-based virtual machines |
| AMI | Template used to launch EC2 instances |
| Instance Types | Different combinations of CPU, RAM, and networking |
| EBS | Persistent block storage for instances |
| Security Groups | Control inbound and outbound traffic |
| Key Pairs | Authenticate connections to instances |
| Auto Scaling | Automatically adjusts the number of instances |
| Load Balancing | Distributes incoming traffic across targets |
| Elastic IP | Static public IPv4 address |
| User Data | Runs startup scripts when an instance launches |

## 4. How EC2 Works

EC2 allows users to launch virtual machines using selected configurations.

### Basic Workflow

1. Select an Amazon Machine Image (AMI).
2. Choose an instance type.
3. Configure a key pair.
4. Configure network settings and security groups.
5. Select storage.
6. Launch the instance.
7. Connect to the instance.
8. Install and run your application.

### Example Architecture

```text
       User
         |
         v
    AWS Console
         |
         v
    Select AMI
         |
         v
  Choose Instance Type
         |
         v
 Configure Security Group
         |
         v
    Select Storage
         |
         v
    Launch EC2
         |
         v
  Connect to Instance
         |
         v
   Run Application
```

## 5. EC2 Instance Types

An EC2 instance type determines the computing resources available to your virtual machine.

Different instance families are designed for different workloads.

### Main Instance Families

| Instance Family | Purpose | Example Use Case |
|---|---|---|
| General Purpose | Balanced CPU and RAM | Web applications |
| Compute Optimized | High CPU performance | ML inference, batch processing |
| Memory Optimized | Large memory capacity | In-memory databases |
| Storage Optimized | High storage performance | Data-intensive applications |
| Accelerated Computing | GPUs and specialized hardware | Deep Learning, AI workloads |

### Examples

- `t3.micro` – General-purpose, burstable instance.
- `t3.small` – General-purpose instance with more resources.
- `c7i.large` – Compute-optimized instance.
- `r7i.large` – Memory-optimized instance.
- `g6.xlarge` – GPU-based instance.

**Note:** Instance availability and pricing depend on the AWS Region. Always check the current pricing before launching an instance.

## 6. Important EC2 Components

### 6.1 AMI (Amazon Machine Image)

An AMI is a template used to create EC2 instances.

It contains the information required to launch a virtual machine, including an operating system and potentially preinstalled software and configuration.

**Examples:**
- Amazon Linux
- Ubuntu Server
- Windows Server
- Red Hat Enterprise Linux

**Why do we use AMI?**

- To launch instances with predefined configurations.
- To create multiple instances with similar environments.
- To create reusable machine templates.

### 6.2 Instance Type

An instance type defines the hardware resources allocated to an EC2 instance.

It determines:

- CPU
- RAM
- Network performance
- Storage options

### 6.3 EBS (Elastic Block Store)

Amazon EBS provides persistent block storage for EC2 instances.

Think of EBS as a virtual hard disk attached to your EC2 instance.

**Example:**

If your EC2 instance is a computer, EBS acts like its hard drive.

**Types of EBS Volumes:**

| Volume Type | Purpose |
|---|---|
| gp3 | General-purpose SSD |
| gp2 | Previous-generation general-purpose SSD |
| io2 | High-performance SSD |
| st1 | Throughput-optimized HDD |
| sc1 | Cold HDD |

### 6.4 Security Groups

Security groups act as virtual firewalls for EC2 instances.

They control inbound and outbound network traffic.

**Inbound Rules:** Control traffic entering the instance.

**Outbound Rules:** Control traffic leaving the instance.

Example:

| Type | Port | Purpose |
|---|---:|---|
| SSH | 22 | Linux remote access |
| RDP | 3389 | Windows remote access |
| HTTP | 80 | Web traffic |
| HTTPS | 443 | Secure web traffic |
| Custom TCP | 8000 | FastAPI application |

**Important:** Never expose SSH or RDP to the entire internet (`0.0.0.0/0`) unnecessarily. Restrict access to trusted IP addresses or use safer access methods.

### 6.5 Key Pairs

A key pair is used to authenticate connections to EC2 instances.

It consists of:

- Public Key: Stored on the EC2 instance.
- Private Key: Kept securely by the user.

For Linux instances, SSH commonly uses the private key file (`.pem`) to authenticate.

**Important:** Never share or upload your private key to GitHub.

### 6.6 Elastic IP

An Elastic IP is a static public IPv4 address that can be associated with an EC2 instance.

It is useful when you need a consistent public IP address.

Note that public IPv4 addresses, including Elastic IPs, can incur charges.

### 6.7 User Data

User Data allows you to run startup scripts when an EC2 instance launches.

Example for Amazon Linux:

```bash
#!/bin/bash
dnf update -y
dnf install -y httpd
systemctl enable httpd
systemctl start httpd
echo "Hello from EC2!" > /var/www/html/index.html
```

This script installs and starts the Apache web server and creates a basic webpage.

## 7. How to Launch an EC2 Instance

Let's create your first EC2 instance using the AWS Management Console.

### Step 1: Open EC2

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com/).
2. Search for EC2.
3. Open the EC2 dashboard.
4. Select your preferred AWS Region.

### Step 2: Launch Instance

1. Click **Launch instance**.
2. Enter an instance name, such as `My-First-EC2`.
3. Select an AMI.

For beginners, you can select Amazon Linux.

### Step 3: Choose Instance Type

Select an instance type suitable for your task.

For basic learning, choose an eligible Free Tier offering if available to your account and Region.

Always verify the current pricing and eligibility.

### Step 4: Create a Key Pair

1. Click Create new key pair.
2. Enter a name, such as `my-ec2-key`.
3. Select RSA.
4. Select `.pem` format for SSH.
5. Download the private key.

Keep the downloaded key in a secure location.

### Step 5: Configure Network Settings

Configure the security group.

For a basic Linux SSH connection:

- Type: SSH
- Port: 22
- Source: My IP

Avoid allowing SSH access from anywhere.

### Step 6: Configure Storage

Choose the required EBS storage size.

For a basic practice instance, the default storage configuration may be sufficient.

### Step 7: Launch Instance

1. Review all configurations.
2. Click Launch instance.
3. Open the Instances page.
4. Wait until the instance reaches the Running state and passes its status checks.

Congratulations! Your first EC2 instance is running.

**Cost Warning:** EC2 instances, EBS volumes, public IPv4 addresses, and other associated resources may incur charges. Check the AWS Pricing Calculator and your account's Free Tier eligibility before launching.

## 8. How to Connect to an EC2 Instance

After launching an instance, you can connect to it remotely.

### 8.1 Connect Using EC2 Instance Connect

1. Open the EC2 dashboard.
2. Select your running Linux instance.
3. Click Connect.
4. Select EC2 Instance Connect if supported.
5. Click Connect.

A browser-based terminal will open.

### 8.2 Connect Using SSH (Windows PowerShell)

For a Linux instance, open PowerShell.

Navigate to the directory containing your `.pem` file.

Example:

```powershell
cd Downloads
```

Connect using:

```bash
ssh -i "my-ec2-key.pem" ec2-user@YOUR_PUBLIC_IP
```

For Ubuntu AMIs, the default username is commonly `ubuntu`.

Replace `YOUR_PUBLIC_IP` with the instance's actual public IP address.

### 8.3 Connect to Windows EC2

Windows EC2 instances generally use Remote Desktop Protocol (RDP).

1. Select your Windows instance.
2. Click Connect.
3. Choose RDP Client.
4. Retrieve the administrator password using the private key.
5. Connect using Remote Desktop.

## 9. EC2 Instance Lifecycle

An EC2 instance has different states during its lifecycle.

| State | Description |
|---|---|
| Pending | Instance is starting |
| Running | Instance is active |
| Stopping | Instance is shutting down |
| Stopped | Instance is powered off |
| Shutting-down | Instance is being terminated |
| Terminated | Instance has been permanently deleted |

### Important Operations

**Start:** Starts a stopped instance.

**Stop:** Shuts down an instance without terminating it. Attached EBS volumes generally remain.

**Reboot:** Restarts the instance.

**Terminate:** Permanently deletes the instance.

**Important:** Terminating an instance may delete its root EBS volume if the volume is configured for deletion on termination. Other attached resources may remain and continue generating charges.

## 10. EC2 Pricing

AWS provides different pricing models for EC2.

### 10.1 On-Demand Instances

Pay for compute capacity according to usage without long-term commitments.

Best for unpredictable or short-term workloads.

### 10.2 Reserved Instances

Commit to a specific instance configuration for a one-year or three-year term in exchange for discounted pricing.

Best for predictable workloads.

### 10.3 Spot Instances

Use spare AWS compute capacity at discounted prices, but AWS can interrupt the instance.

Best for fault-tolerant workloads and batch processing.

### 10.4 Savings Plans

Commit to a consistent amount of eligible compute usage per hour for one or three years in exchange for lower rates.

### Pricing Comparison

| Model | Main Advantage |
|---|---|
| On-Demand | Flexible usage |
| Reserved Instances | Commitment-based discounts |
| Spot | Lower-cost spare capacity |
| Savings Plans | Flexible commitment-based discounts |

**Note:** Total EC2 costs can include compute, EBS storage, data transfer, public IPv4 addresses, and other associated resources.

## 11. Practical EC2 Commands Using AWS CLI

AWS CLI allows you to manage EC2 instances directly from your terminal.

### Check AWS CLI Installation

```bash
aws --version
```

### Check Current AWS Identity

```bash
aws sts get-caller-identity
```

### List EC2 Instances

```bash
aws ec2 describe-instances
```

### List Instances with IDs and States

```bash
aws ec2 describe-instances \
  --query "Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]" \
  --output table
```

### Start an Instance

```bash
aws ec2 start-instances --instance-ids INSTANCE_ID
```

### Stop an Instance

```bash
aws ec2 stop-instances --instance-ids INSTANCE_ID
```

### Reboot an Instance

```bash
aws ec2 reboot-instances --instance-ids INSTANCE_ID
```

### Terminate an Instance

```bash
aws ec2 terminate-instances --instance-ids INSTANCE_ID
```

**Warning:** The terminate command permanently terminates the specified instance. Verify the instance ID before running it.

## 12. Real-World Use Cases

### 12.1 Web Application Hosting

Deploy websites and web applications on EC2 instances.

### 12.2 Data Science

- Run Python scripts.
- Perform data preprocessing.
- Execute large data analysis tasks.
- Host Jupyter environments.

### 12.3 Machine Learning

- Train machine learning models.
- Run model inference.
- Deploy ML APIs using FastAPI.
- Use GPU-based instances for deep learning.

### 12.4 Data Engineering

- Run ETL scripts.
- Process datasets.
- Schedule data processing jobs.
- Connect applications to databases.

### 12.5 MLOps

- Host model-serving applications.
- Deploy ML pipelines.
- Run Docker containers.
- Integrate applications with CI/CD pipelines.

## 13. EC2 Best Practices

1. **Choose the Right Instance:** Select an instance type according to CPU, memory, and workload requirements.
2. **Use IAM Roles:** Attach IAM roles to EC2 instances instead of storing long-term AWS access keys.
3. **Secure Security Groups:** Allow only the necessary ports and trusted IP addresses.
4. **Use SSH Keys Securely:** Never share or commit private keys to GitHub.
5. **Monitor Resources:** Use Amazon CloudWatch to monitor CPU utilization, network activity, and other metrics.
6. **Stop Unused Instances:** Stop instances when they are not required.
7. **Terminate Unused Resources:** Remove resources that are no longer needed.
8. **Use EBS Snapshots:** Back up important EBS volumes.
9. **Apply Updates:** Keep operating systems and applications updated.
10. **Monitor Costs:** Use AWS Budgets and billing alerts to track spending.

## 14. Common EC2 Errors

### 1. Connection Timeout

**Possible Causes:**
- Security group does not allow the required port.
- Incorrect public IP address.
- Instance is not running.
- Network configuration blocks the connection.

**Solution:** Verify instance status, public IP, security group rules, and network connectivity.

### 2. Permission Denied (SSH)

**Possible Causes:**
- Incorrect username.
- Wrong private key.
- Incorrect key permissions.

**Solution:** Verify the key pair and use the correct default username for your AMI.

### 3. Instance Failed to Launch

**Possible Causes:**
- Insufficient capacity.
- Instance type unavailable in the selected Availability Zone.
- Service quotas reached.

**Solution:** Check the error message and try another supported instance type or Availability Zone.

### 4. Instance Stopped Unexpectedly

**Possible Causes:**
- Manual stop operation.
- Operating system shutdown.
- AWS-initiated interruption, such as a Spot interruption.

**Solution:** Check instance state transition details and relevant system logs.

### 5. Unexpected AWS Bill

**Possible Causes:**
- Running instances.
- Unused EBS volumes.
- Allocated public IPv4 addresses.
- Snapshots and data transfer.

**Solution:** Review the AWS Billing and Cost Management dashboard and clean up unused resources.

## 15. Interview Questions

**Q1. What is Amazon EC2?**

Amazon EC2 is an AWS compute service that provides scalable virtual servers in the cloud.

**Q2. What is an EC2 instance?**

An EC2 instance is a virtual machine running on AWS infrastructure.

**Q3. What is an AMI?**

An Amazon Machine Image is a template containing the information required to launch an EC2 instance.

**Q4. What is the difference between AMI and EC2 instance?**

AMI is the template used to launch a virtual machine, whereas an EC2 instance is the running virtual machine.

**Q5. What is EBS?**

Elastic Block Store provides persistent block storage volumes for EC2 instances.

**Q6. What is a Security Group?**

A security group is a virtual firewall that controls inbound and outbound traffic for associated resources.

**Q7. What is the difference between Stop and Terminate?**

Stop shuts down an instance while generally retaining its attached EBS volumes. Terminate permanently deletes the instance and may delete its root volume depending on its configuration.

**Q8. What is a Key Pair?**

A key pair consists of a public key and private key used to authenticate connections to an EC2 instance.

**Q9. What is an Elastic IP?**

An Elastic IP is a static public IPv4 address that can be associated with an AWS resource.

**Q10. What is Auto Scaling?**

Auto Scaling automatically adjusts the number of EC2 instances based on configured conditions and scaling policies.

**Q11. What is the difference between On-Demand and Spot Instances?**

On-Demand instances provide flexible compute capacity at standard rates, while Spot Instances use spare AWS capacity at discounted rates and can be interrupted.

**Q12. What is User Data in EC2?**

User Data allows us to execute startup scripts when an EC2 instance launches.

**Q13. How can we securely connect to an EC2 instance?**

We can use SSH with key-based authentication for Linux or RDP for Windows. Other options include EC2 Instance Connect and AWS Systems Manager Session Manager, where supported and configured.

**Q14. Can an EC2 instance have multiple EBS volumes?**

Yes. An EC2 instance can have multiple EBS volumes attached, subject to instance and service limits.

**Q15. What is the difference between EC2 and Lambda?**

EC2 provides virtual servers with control over the operating system and environment. Lambda runs code in a serverless environment without requiring users to manage servers.

## 16. Quick Revision

| Term | Meaning |
|---|---|
| EC2 | Elastic Compute Cloud |
| AMI | Amazon Machine Image |
| EBS | Elastic Block Store |
| SG | Security Group |
| Key Pair | Public and private key used for authentication |
| Elastic IP | Static public IPv4 address |
| User Data | Startup scripts |
| Auto Scaling | Automatically adjusts instance capacity |
| On-Demand | Pay for usage without long-term commitment |
| Spot | Uses spare AWS compute capacity |
| SSH | Secure Shell |
| RDP | Remote Desktop Protocol |

