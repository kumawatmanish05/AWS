# Amazon EBS (Elastic Block Store)

## Table of Contents

1. [What is Amazon EBS?](#1-what-is-amazon-ebs)
2. [Why Do We Use EBS?](#2-why-do-we-use-ebs)
3. [Features of EBS](#3-features-of-ebs)
4. [How EBS Works](#4-how-ebs-works)
5. [EBS Volume Types](#5-ebs-volume-types)
6. [Important EBS Components](#6-important-ebs-components)
7. [How to Create and Attach an EBS Volume](#7-how-to-create-and-attach-an-ebs-volume)
8. [How to Mount an EBS Volume](#8-how-to-mount-an-ebs-volume)
9. [EBS Snapshots](#9-ebs-snapshots)
10. [EBS Volume Lifecycle](#10-ebs-volume-lifecycle)
11. [EBS Pricing](#11-ebs-pricing)
12. [Practical EBS Commands Using AWS CLI](#12-practical-ebs-commands-using-aws-cli)
13. [Real-World Use Cases](#13-real-world-use-cases)
14. [EBS Best Practices](#14-ebs-best-practices)
15. [Common EBS Errors](#15-common-ebs-errors)
16. [Interview Questions](#16-interview-questions)
17. [Quick Revision](#17-quick-revision)

---

## 1. What is Amazon EBS?

Amazon EBS stands for **Amazon Elastic Block Store**.

It is a block-level storage service provided by AWS that allows us to attach persistent storage volumes to EC2 instances.

EBS works like a virtual hard drive for an EC2 instance.

It stores data such as operating systems, application files, databases, and other information.

**Example:**

Suppose you launch an EC2 instance to run a Python application. The instance needs storage to save application files and datasets.

You can attach an EBS volume to the EC2 instance and use it to store that data.

### Key Points

- EBS provides block-level storage for EC2 instances.
- EBS volumes are persistent storage.
- Volumes are generally available within a specific Availability Zone.
- You can increase volume size and modify certain performance settings.
- EBS supports snapshots for backups.
- EBS volumes can generally be detached and reattached to compatible instances.
- EBS is commonly used for databases, applications, and operating system storage.

## 2. Why Do We Use EBS?

EBS is used when EC2 instances need reliable and persistent storage.

### Main Reasons

1. **Persistent Storage:** Data can remain available even when an EC2 instance is stopped.
2. **Scalability:** Increase storage capacity as requirements grow.
3. **Data Backup:** Create snapshots to back up volume data.
4. **Performance:** Select volume types according to workload requirements.
5. **Flexibility:** Attach and detach volumes from compatible EC2 instances.
6. **Reliability:** EBS volumes are designed for high availability within their Availability Zone.
7. **Data Recovery:** Restore volumes from snapshots when required.

## 3. Features of EBS

| Feature | Description |
|---|---|
| Block Storage | Stores data in blocks |
| Persistent Storage | Data generally remains after EC2 stops |
| Snapshots | Creates point-in-time backups |
| Encryption | Supports encryption at rest and in transit |
| Scalability | Allows volume size modification |
| Performance | Provides different IOPS and throughput options |
| Detach and Attach | Move volumes between compatible instances |
| Monitoring | Monitor performance using CloudWatch |

## 4. How EBS Works

EBS creates storage volumes that can be attached to EC2 instances.

Once attached, the operating system can access the volume as a block device.

### Basic Workflow

1. Launch an EC2 instance.
2. Create an EBS volume.
3. Select the Availability Zone.
4. Attach the volume to the EC2 instance.
5. Format the volume with a filesystem if it is new.
6. Mount the volume.
7. Store and access data.

### Example Architecture

```text
       AWS Cloud
           |
           v
      EC2 Instance
           |
           v
      EBS Volume
           |
           v
    File System
           |
           v
    Store Application
         Data
```

**Important:** An EBS volume and its EC2 instance must be in the same Availability Zone for normal attachment.

## 5. EBS Volume Types

EBS provides different volume types for different workloads.

### 5.1 General Purpose SSD

Designed for workloads requiring balanced price and performance.

**Types:**
- gp3
- gp2

**Use Cases:**
- Boot volumes
- Development environments
- Small and medium databases
- General-purpose applications

**Recommendation:** gp3 is a common default choice for general-purpose SSD workloads.

### 5.2 Provisioned IOPS SSD

Designed for workloads requiring high and consistent I/O performance.

**Types:**
- io2 Block Express
- io1

**Use Cases:**
- Large databases
- Critical business applications
- High-performance transactional workloads

### 5.3 Throughput Optimized HDD

Designed for workloads requiring high sequential throughput.

**Type:**
- st1

**Use Cases:**
- Big data processing
- Log processing
- Data warehouses
- Frequently accessed sequential data

### 5.4 Cold HDD

Designed for infrequently accessed data requiring lower-cost storage.

**Type:**
- sc1

**Use Cases:**
- Infrequently accessed datasets
- Large sequential data storage
- Lower-cost archival-style workloads

### Volume Type Comparison

| Volume Type | Storage | Best For |
|---|---|---|
| gp3 | General Purpose SSD | General applications |
| gp2 | General Purpose SSD | General workloads |
| io2 | Provisioned IOPS SSD | Critical databases |
| io1 | Provisioned IOPS SSD | High-performance workloads |
| st1 | Throughput Optimized HDD | Big data and sequential workloads |
| sc1 | Cold HDD | Infrequently accessed data |

**Note:** HDD volume types cannot be used as boot volumes. Check current AWS documentation for supported volume limits and performance specifications.

## 6. Important EBS Components

### 6.1 EBS Volume

An EBS volume is a virtual storage device that can be attached to an EC2 instance.

It stores data independently of the instance's temporary compute lifecycle.

### 6.2 EBS Snapshot

An EBS snapshot is a point-in-time backup of an EBS volume.

Snapshots are stored in Amazon S3-managed infrastructure, but users manage them through the EBS snapshot interface.

**Why do we use snapshots?**

- Back up important data.
- Restore data after accidental deletion.
- Create new volumes from backups.
- Copy backups across Regions.

### 6.3 IOPS

IOPS stands for Input/Output Operations Per Second.

It measures how many read and write operations a storage volume can perform per second.

Higher IOPS is important for workloads with many small, random read/write operations.

### 6.4 Throughput

Throughput measures how much data a storage volume can transfer per second.

It is commonly measured in MiB/s.

It is important for workloads involving large sequential reads and writes.

### 6.5 Volume Size

EBS volume size represents the storage capacity allocated to a volume.

You can modify the size of supported volumes without recreating the entire volume.

### 6.6 EBS Encryption

EBS encryption protects data stored on volumes and snapshots.

It uses AWS Key Management Service (KMS) keys for encryption.

Benefits:
- Protects data at rest.
- Supports encrypted snapshots.
- Helps secure sensitive application data.

## 7. How to Create and Attach an EBS Volume

Let's create an EBS volume and attach it to an EC2 instance.

### Step 1: Open EC2

1. Sign in to the AWS Management Console.
2. Search for EC2.
3. Open the EC2 dashboard.
4. Select your preferred AWS Region.

### Step 2: Open Volumes

1. In the left navigation panel, find Elastic Block Store.
2. Click Volumes.
3. Click Create volume.

### Step 3: Configure Volume

Choose the following settings:

- Volume Type: gp3
- Size: 5 GiB (for basic practice)
- Availability Zone: Same as your EC2 instance
- Encryption: Enable encryption

### Step 4: Create Volume

1. Review the configuration.
2. Click Create volume.
3. Wait until the volume status becomes Available.

### Step 5: Attach Volume

1. Select the newly created volume.
2. Click Actions.
3. Select Attach volume.
4. Choose your EC2 instance.
5. Enter a device name if required.
6. Click Attach volume.

The EBS volume is now attached to your EC2 instance.

**Important:** Attaching a volume does not automatically make its filesystem available. You must format and mount a new volume inside the operating system.

## 8. How to Mount an EBS Volume

After attaching an EBS volume, you need to prepare it for use inside the operating system.

The following example uses a Linux EC2 instance.

### Step 1: Connect to EC2

Connect to your Linux EC2 instance using SSH or EC2 Instance Connect.

### Step 2: Identify the Attached Volume

Run:

```bash
lsblk
```

This command displays available block devices.

Example output:

```text
NAME        MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0   8G  0 disk
├─nvme0n1p1 259:1    0   8G  0 part /
nvme1n1     259:2    0   5G  0 disk
```

In this example, `nvme1n1` is the newly attached volume.

**Important:** Device names vary by instance type and operating system. Always identify the correct device before formatting it.

### Step 3: Create a Filesystem

For a brand-new, empty volume only:

```bash
sudo mkfs -t xfs /dev/nvme1n1
```

This creates an XFS filesystem.

**Warning:** Formatting destroys existing filesystem data. Never run this on a volume that contains data you need.

### Step 4: Create a Mount Directory

```bash
sudo mkdir /data
```

### Step 5: Mount the Volume

```bash
sudo mount /dev/nvme1n1 /data
```

### Step 6: Verify the Mount

```bash
df -h
```

The output should show the mounted volume at `/data`.

### Step 7: Create a Test File

```bash
echo "Hello from EBS" | sudo tee /data/test.txt
```

Check the file:

```bash
cat /data/test.txt
```

Your EBS volume is now ready to store data.

**Note:** The mount command above does not automatically mount the volume after a reboot. For permanent mounting, configure `/etc/fstab` using the volume's UUID and test the configuration carefully.

## 9. EBS Snapshots

EBS snapshots are point-in-time backups of EBS volumes.

They are useful for protecting important data and recovering from failures.

### How to Create a Snapshot

1. Open the EC2 dashboard.
2. Navigate to Elastic Block Store.
3. Select Volumes.
4. Select the volume you want to back up.
5. Click Actions.
6. Select Create snapshot.
7. Enter a description.
8. Click Create snapshot.

### How to Restore from a Snapshot

1. Open the Snapshots section.
2. Select the snapshot.
3. Click Actions.
4. Select Create volume from snapshot.
5. Choose the required Availability Zone.
6. Create the new volume.
7. Attach it to a compatible EC2 instance.

### Snapshot Benefits

- Data backup.
- Disaster recovery.
- Volume restoration.
- Cross-Region backup copies.
- Creating new volumes from existing backups.

**Important:** Snapshots are incremental after the initial snapshot. Snapshot storage and related operations may incur charges.

## 10. EBS Volume Lifecycle

An EBS volume has different states during its lifecycle.

| State | Description |
|---|---|
| Creating | Volume is being created |
| Available | Volume is ready for attachment |
| In-use | Volume is attached to an instance |
| Deleting | Volume is being deleted |
| Deleted | Volume has been deleted |
| Error | Volume creation or operation failed |

### Important Operations

**Create:** Creates a new EBS volume.

**Attach:** Connects a volume to an EC2 instance.

**Detach:** Disconnects a volume from an instance.

**Modify:** Changes supported volume settings, such as size and performance.

**Snapshot:** Creates a backup of the volume.

**Delete:** Permanently deletes the volume.

**Important:** Before detaching a volume, unmount its filesystem safely from the operating system. Deleting a volume permanently removes its data unless you have a backup.

## 11. EBS Pricing

AWS charges for EBS resources based on their type, size, performance configuration, and usage.

### Main Pricing Factors

1. **Volume Storage:** Charges depend on allocated storage.
2. **Provisioned IOPS:** Certain volume types charge for provisioned performance.
3. **Throughput:** Additional throughput configurations may affect pricing.
4. **Snapshots:** Snapshot storage incurs charges.
5. **Data Transfer:** Certain data transfer operations may incur charges.

### Important Points

- Stopping an EC2 instance does not automatically delete its EBS volumes.
- EBS volumes can continue generating storage charges when detached.
- Snapshots may continue generating storage charges.
- Deleting an EC2 instance does not necessarily delete every attached EBS volume.
- Always review your AWS billing dashboard.

## 12. Practical EBS Commands Using AWS CLI

AWS CLI allows you to manage EBS volumes directly from your terminal.

### Check AWS CLI Version

```bash
aws --version
```

### List EBS Volumes

```bash
aws ec2 describe-volumes
```

### Create an EBS Volume

Replace the Availability Zone with one matching your intended EC2 instance.

```bash
aws ec2 create-volume \
  --availability-zone ap-south-1a \
  --size 5 \
  --volume-type gp3 \
  --encrypted
```

### Attach an EBS Volume

```bash
aws ec2 attach-volume \
  --volume-id vol-xxxxxxxx \
  --instance-id i-xxxxxxxx \
  --device /dev/sdf
```

### Detach an EBS Volume

```bash
aws ec2 detach-volume \
  --volume-id vol-xxxxxxxx
```

Unmount the filesystem safely before detaching.

### Create a Snapshot

```bash
aws ec2 create-snapshot \
  --volume-id vol-xxxxxxxx \
  --description "EBS Backup"
```

### List Snapshots

```bash
aws ec2 describe-snapshots --owner-ids self
```

### Delete an EBS Volume

```bash
aws ec2 delete-volume \
  --volume-id vol-xxxxxxxx
```

**Warning:** Replace placeholder IDs with actual resource IDs. Verify that a volume is not needed before deleting it.

## 13. Real-World Use Cases

### 13.1 Database Storage

- Store database files.
- Support transactional workloads.
- Provide persistent database storage.

### 13.2 Data Science

- Store large datasets.
- Save processed data.
- Store model files and experiment outputs.
- Maintain Jupyter Notebook environments.

### 13.3 Machine Learning

- Store training datasets.
- Save trained model weights.
- Store checkpoints.
- Support high-performance ML workloads.

### 13.4 Data Engineering

- Store intermediate ETL files.
- Maintain processing data.
- Store application logs.
- Support data processing applications.

### 13.5 MLOps

- Store model artifacts.
- Maintain application files.
- Store deployment-related data.
- Support persistent storage for applications running on EC2.

## 14. EBS Best Practices

1. **Enable Encryption:** Encrypt volumes and snapshots containing sensitive data.
2. **Create Snapshots:** Regularly back up important volumes.
3. **Choose the Right Volume Type:** Select gp3, io2, or HDD based on workload requirements.
4. **Monitor Performance:** Use CloudWatch to monitor volume performance.
5. **Avoid Unused Volumes:** Delete volumes that are no longer required.
6. **Use Appropriate IOPS:** Configure performance according to application requirements.
7. **Check Availability Zones:** Ensure the volume and EC2 instance are in the same Availability Zone.
8. **Use IAM Permissions:** Follow least-privilege access when managing EBS resources.
9. **Monitor Costs:** Review storage and snapshot charges.
10. **Test Backups:** Verify that snapshots can successfully restore volumes.

## 15. Common EBS Errors

### 1. Volume and Instance in Different Availability Zones

**Cause:** EBS volume and EC2 instance are located in different Availability Zones.

**Solution:** Create the volume in the same Availability Zone as the EC2 instance.

### 2. Volume Cannot Be Attached

**Possible Causes:**
- Volume is already attached.
- Instance or volume state is incorrect.
- Attachment limits have been reached.

**Solution:** Check volume state, instance state, and attachment limits.

### 3. Volume Not Visible in Linux

**Possible Causes:**
- Device name differs from the expected name.
- Attachment is not complete.
- Operating system has not detected the device.

**Solution:** Run `lsblk` and check the instance's storage devices.

### 4. Filesystem Not Mounting

**Possible Causes:**
- Filesystem is not created.
- Incorrect device path.
- Filesystem corruption.

**Solution:** Verify the device and filesystem before attempting repair.

### 5. Unable to Delete Volume

**Possible Causes:**
- Volume is still attached.
- Volume is in an incompatible state.

**Solution:** Safely unmount and detach the volume before deletion.

### 6. Unexpected EBS Charges

**Possible Causes:**
- Unused volumes.
- Old snapshots.
- Oversized volumes.
- Provisioned performance settings.

**Solution:** Review EBS volumes, snapshots, and billing details.

## 16. Interview Questions

**Q1. What is Amazon EBS?**

Amazon Elastic Block Store is a block-level storage service that provides persistent storage volumes for EC2 instances.

**Q2. What is the difference between EBS and S3?**

EBS provides block-level storage for EC2 instances, whereas S3 provides object storage for files and data objects.

**Q3. What is an EBS volume?**

An EBS volume is a virtual block storage device that can be attached to an EC2 instance.

**Q4. What is an EBS snapshot?**

An EBS snapshot is a point-in-time backup of an EBS volume.

**Q5. What is the difference between gp3 and io2?**

gp3 is a general-purpose SSD volume, whereas io2 is designed for workloads requiring high and consistent IOPS.

**Q6. What is IOPS?**

IOPS means Input/Output Operations Per Second. It measures the number of storage read and write operations performed per second.

**Q7. What is the difference between EBS and Instance Store?**

EBS provides persistent block storage, whereas Instance Store provides temporary storage physically associated with the host machine.

**Q8. Can an EBS volume be attached to multiple EC2 instances?**

Certain EBS volume types support Multi-Attach, subject to supported instance and operating system configurations. Most EBS volumes are attached to one instance at a time.

**Q9. Can we resize an EBS volume?**

Yes. Supported EBS volumes can be modified to increase their size and adjust certain performance settings.

**Q10. What happens to EBS data when an EC2 instance stops?**

EBS data generally remains intact when an EC2 instance is stopped.

**Q11. Can EBS volumes be shared across Availability Zones?**

No. An EBS volume is associated with a single Availability Zone. To use its data in another Availability Zone, create a snapshot and restore it there.

**Q12. What is the difference between EBS and EFS?**

EBS provides block storage for EC2 instances, whereas EFS provides managed, shared file storage that can be accessed by multiple clients.

**Q13. What is EBS encryption?**

EBS encryption protects data stored on EBS volumes and snapshots using AWS KMS keys.

**Q14. What happens when an EBS volume is deleted?**

The volume and its stored data are permanently deleted. Recovering the data requires a suitable backup or snapshot.

**Q15. What is the purpose of EBS Multi-Attach?**

It allows a supported EBS volume to be attached to multiple compatible EC2 instances simultaneously.

## 17. Quick Revision

| Term | Meaning |
|---|---|
| EBS | Elastic Block Store |
| Volume | Virtual block storage device |
| Snapshot | Point-in-time volume backup |
| gp3 | General-purpose SSD |
| io2 | Provisioned IOPS SSD |
| st1 | Throughput Optimized HDD |
| sc1 | Cold HDD |
| IOPS | Input/Output Operations Per Second |
| Throughput | Data transfer rate |
| KMS | Key Management Service |
| Attach | Connect volume to EC2 |
| Detach | Disconnect volume from EC2 |
| Mount | Make filesystem accessible |
| Multi-Attach | Attach supported volumes to multiple instances |


---

## Additional Learning Resources

- [Official AWS EBS Documentation](https://docs.aws.amazon.com/ebs/)
- [EBS Volume Types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)
- [Create an EBS Volume](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-creating-volume.html)
- [Attach an EBS Volume](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-attaching-volume.html)
- [EBS Snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html)
- [EBS Pricing](https://aws.amazon.com/ebs/pricing/)

**Learning Goal:** Understand how to create, attach, configure, mount, back up, and manage persistent storage using Amazon EBS.
