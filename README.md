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
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Partitioning and Transferring an EBS Volume to Another EC2 Instance</title>
</head>
<body>
    <h1>Partitioning and Transferring an EBS Volume to Another EC2 Instance</h1>

    <h2>Step 1: Partition the Volume</h2>
    <ol>
        <li><strong>Partition the Volume</strong>
            <pre><code>sudo fdisk /dev/xvdf</code></pre>
            Create a 7 GB, 5 GB, and 3 GB partition.
        </li>
        <li><strong>Format the Partitions</strong>
            <pre><code>sudo mkfs.ext4 /dev/xvdf1
sudo mkfs.ext4 /dev/xvdf2
sudo mkfs.ext4 /dev/xvdf3</code></pre>
        </li>
        <li><strong>Mount the Partitions</strong>
            <pre><code>sudo mkdir -p /mnt/vol1 /mnt/vol2 /mnt/vol3
sudo mount /dev/xvdf1 /mnt/vol1
sudo mount /dev/xvdf2 /mnt/vol2
sudo mount /dev/xvdf3 /mnt/vol3</code></pre>
        </li>
    </ol>

    <h2>Step 2: Install Apache and Other Necessary Software</h2>
    <ol>
        <li><strong>Update the System</strong>
            <ul>
                <li>For Amazon Linux:
                    <pre><code>sudo yum update -y</code></pre>
                </li>
                <li>For Ubuntu:
                    <pre><code>sudo apt update -y</code></pre>
                </li>
            </ul>
        </li>
        <li><strong>Install Apache</strong>
            <ul>
                <li>For Amazon Linux:
                    <pre><code>sudo yum install httpd -y</code></pre>
                </li>
                <li>For Ubuntu:
                    <pre><code>sudo apt install apache2 -y</code></pre>
                </li>
            </ul>
        </li>
        <li><strong>Start and Enable Apache</strong>
            <ul>
                <li>For Amazon Linux:
                    <pre><code>sudo systemctl start httpd
sudo systemctl enable httpd</code></pre>
                </li>
                <li>For Ubuntu:
                    <pre><code>sudo systemctl start apache2
sudo systemctl enable apache2</code></pre>
                </li>
            </ul>
        </li>
        <li><strong>Verify Apache Installation</strong>
            <p>Open your web browser and navigate to your EC2 instance's public IP address. You should see the Apache default page.</p>
        </li>
    </ol>

    <h2>Step 3: Persist the Mounts Across Reboots</h2>
    <ol>
        <li><strong>Edit `/etc/fstab`</strong>
            <pre><code>sudo nano /etc/fstab</code></pre>
            Add the following lines to mount the partitions automatically on reboot:
            <pre><code>/dev/xvdf1  /mnt/vol1  ext4  defaults,nofail  0  2
/dev/xvdf2  /mnt/vol2  ext4  defaults,nofail  0  2
/dev/xvdf3  /mnt/vol3  ext4  defaults,nofail  0  2</code></pre>
        </li>
        <li><strong>Test the Configuration</strong>
            <pre><code>sudo mount -a</code></pre>
        </li>
    </ol>

    <h2>Step 4: Detach the Volume from the Current Instance</h2>
    <ol>
        <li><strong>Safely Unmount the Volume</strong>
            <pre><code>sudo umount /mnt/vol1
sudo umount /mnt/vol2
sudo umount /mnt/vol3</code></pre>
        </li>
        <li><strong>Detach the Volume via AWS Management Console</strong>
            <ul>
                <li>Go to the AWS Management Console.</li>
                <li>Navigate to the EC2 Dashboard.</li>
                <li>Select "Volumes" from the left-hand menu.</li>
                <li>Find the volume you wish to detach.</li>
                <li>Click on "Actions" &gt; "Detach Volume".</li>
                <li>Confirm the detachment.</li>
            </ul>
        </li>
    </ol>

    <h2>Step 5: Attach the Volume to a New EC2 Instance</h2>
    <ol>
        <li><strong>Attach the Volume via AWS Management Console</strong>
            <ul>
                <li>In the EC2 Dashboard, under "Volumes", find the detached volume.</li>
                <li>Click on "Actions" &gt; "Attach Volume".</li>
                <li>Select the new EC2 instance from the dropdown menu.</li>
                <li>Specify the device name (e.g., /dev/xvdf).</li>
                <li>Click "Attach".</li>
            </ul>
        </li>
        <li><strong>Verify the Volume Attachment</strong>
            <pre><code>lsblk</code></pre>
            You should see the attached volume listed (e.g., /dev/xvdf).
        </li>
    </ol>

    <h2>Step 6: Mount the Partitions on the New Instance</h2>
    <ol>
        <li><strong>Create Mount Points</strong>
            <pre><code>sudo mkdir -p /mnt/vol1 /mnt/vol2 /mnt/vol3</code></pre>
        </li>
        <li><strong>Mount the Partitions</strong>
            <pre><code>sudo mount /dev/xvdf1 /mnt/vol1
sudo mount /dev/xvdf2 /mnt/vol2
sudo mount /dev/xvdf3 /mnt/vol3</code></pre>
        </li>
        <li><strong>Persist the Mounts Across Reboots</strong>
            <pre><code>sudo nano /etc/fstab</code></pre>
            Add the following lines:
            <pre><code>/dev/xvdf1  /mnt/vol1  ext4  defaults,nofail  0  2
/dev/xvdf2  /mnt/vol2  ext4  defaults,nofail  0  2
/dev/xvdf3  /mnt/vol3  ext4  defaults,nofail  0  2</code></pre>
        </li>
        <li><strong>Test the Configuration</strong>
            <pre><code>sudo mount -a</code></pre>
        </li>
    </ol>

    <h2>Step 7: (Optional) Reinstall Apache on the New Instance</h2>
    <p>If Apache was installed on the previous instance and you need it on the new instance as well, follow these steps:</p>
    <ol>
        <li><strong>Install Apache</strong>
            <ul>
                <li>For Amazon Linux:
                    <pre><code>sudo yum install httpd -y</code></pre>
                </li>
                <li>For Ubuntu:
                    <pre><code>sudo apt install apache2 -y</code></pre>
                </li>
            </ul>
        </li>
    </ol>
</body>
</html>
