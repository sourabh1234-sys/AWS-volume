# AWS EBS Volume Management 

## Overview

This project demonstrates the creation of a 15 GB EBS volume on AWS, partitioning it into three segments (7 GB, 5 GB, and 3 GB), installing Apache, and transferring the EBS volume to another EC2 instance.

## Prerequisites

Before you begin, ensure that you have the following:

- An AWS account with access to EC2 and EBS services.
- An EC2 instance running a Linux distribution (e.g., Amazon Linux, Ubuntu).
- Basic knowledge of AWS CLI and Linux command-line interface.

## Steps

### 1. Create and Attach the EBS Volume

1. **Create a 15 GB EBS Volume**:
   - Go to the AWS Management Console.
   - Navigate to the EC2 Dashboard.
   - Select "Volumes" from the left-hand menu.
   - Click on "Create Volume" and set the size to 15 GB.
   - Choose the same availability zone as your EC2 instance.
   - Click on "Create Volume".

2. **Attach the Volume to the EC2 Instance**:
   - Select the newly created volume.
   - Click on "Actions" > "Attach Volume".
   - Select your EC2 instance and click "Attach".

### 2. Partition the EBS Volume

1. **Connect to the EC2 Instance**:
   - Use SSH to connect to your EC2 instance.

2. **Verify the Volume**:
   ```bash
   lsblk
3. **Partition the Volume**:
   -Use fdisk to create three partitions
   sudo fdisk /dev/xvdf
4.**Format the Partitions**:
   sudo mkfs.ext4 /dev/xvdf1
   sudo mkfs.ext4 /dev/xvdf2
   sudo mkfs.ext4 /dev/xvdf3

