# AWS Lab: Managing Amazon EBS Volumes and Snapshots

**Project Duration:** ~45 Minutes  
**Tech Stack:** AWS EC2, Amazon EBS, Linux CLI, Bash Scripting

## Project Overview
In this lab, I explored the fundamentals of block storage on AWS using **Amazon Elastic Block Store (EBS)**. The goal was to understand the lifecycle of a storage volume separate from the compute instance. I practiced creating volumes, attaching them to a running Linux server, formatting file systems, and performing backup and restore operations using Snapshots.

[Image of Amazon EBS volume architecture]

## Key Objectives Achieved
* **Provisioning Storage:** Created and configured General Purpose SSD (gp2) volumes.
* **Linux Administration:** Mounted raw block devices to the Linux file system.
* **Data Persistence:** Configured fstab for persistent mounting across reboots.
* **Disaster Recovery:** Created snapshots and restored data to new volumes.

---

## Step-by-Step Walkthrough

### 1. Provisioning the EBS Volume
I started by navigating to the **EC2 Console** to inspect my running instance (`Lab`). I noted that it was deployed in the `us-west-2a` Availability Zone.

Because EBS volumes are AZ-locked, I created a new **1 GiB gp2 volume** in the exact same zone (`us-west-2a`). I tagged it with `Name: My Volume` to keep my resources organized.

### 2. Attaching the Volume
Once the volume state moved to **Available**, I attached it to the running `Lab` instance. I designated the device name as `/dev/sdb` so I could identify it later in the Linux terminal.

### 3. System Configuration & Mounting
I connected to the instance via **EC2 Instance Connect**. Initially, running `df -h` showed only the root volume. To make the new storage usable, I had to format and mount it.

**Commands used:**
```bash
# 1. Formatted the new volume with the ext3 file system
sudo mkfs -t ext3 /dev/sdb

# 2. Created a mount point directory
sudo mkdir /mnt/data-store

# 3. Mounted the volume
sudo mount /dev/sdb /mnt/data-store
