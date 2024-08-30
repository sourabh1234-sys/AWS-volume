AWS EBS Volume Management and Apache Installation
=============================================

Overview This project demonstrates the creation of a 15 GB EBS volume on
AWS, partitioning it into three segments (7 GB, 5 GB, and 3 GB),
installing Apache, and transferring the EBS volume to another EC2
instance.

Prerequisites Before you begin, ensure that you have the following:

An AWS account with access to EC2 and EBS services. An EC2 instance
running a Linux distribution (e.g., Amazon Linux, Ubuntu). Basic
knowledge of AWS CLI and Linux command-line interface. Steps 1. Create
and Attach the EBS Volume 1.1 Create a 15 GB EBS Volume Go to the AWS
Management Console. Navigate to the EC2 Dashboard. Select \"Volumes\"
from the left-hand menu. Click on \"Create Volume\" and set the size to
15 GB. Choose the same availability zone as your EC2 instance. Click on
\"Create Volume\". 1.2 Attach the Volume to the EC2 Instance Select the
newly created volume. Click on \"Actions\" \> \"Attach Volume\". Select
your EC2 instance and click \"Attach\". 2. Partition the EBS Volume 2.1
Connect to the EC2 Instance Use SSH to connect to your EC2 instance. 2.2
Verify the Volume bash

Verify

Open In Editor Edit Copy code lsblk You should see the new volume (e.g.,
/dev/xvdf).

2.3 Partition the Volume Use fdisk to create three partitions:

bash

Verify

Open In Editor Edit Copy code sudo fdisk /dev/xvdf Create a 7 GB, 5 GB,
and 3 GB partition.

2.4 Format the Partitions bash

Verify

Open In Editor Edit Copy code sudo mkfs.ext4 /dev/xvdf1 sudo mkfs.ext4
/dev/xvdf2 sudo mkfs.ext4 /dev/xvdf3 2.5 Mount the Partitions bash

Verify

Open In Editor Edit Copy code sudo mkdir -p /mnt/vol1 /mnt/vol2
/mnt/vol3 sudo mount /dev/xvdf1 /mnt/vol1 sudo mount /dev/xvdf2
/mnt/vol2 sudo mount /dev/xvdf3 /mnt/vol3 3. Install Apache and Other
Necessary Software 3.1 Update the System bash

Verify

Open In Editor Edit Copy code sudo yum update -y \# For Amazon Linux
sudo apt update -y \# For Ubuntu 3.2 Install Apache bash

Verify

Open In Editor Edit Copy code sudo yum install httpd -y \# For Amazon
Linux sudo apt install apache2 -y \# For Ubuntu 3.3 Start and Enable
Apache bash

Verify

Open In Editor Edit Copy code sudo systemctl start httpd \# Amazon Linux
sudo systemctl enable httpd

sudo systemctl start apache2 \# Ubuntu sudo systemctl enable apache2 3.4
Verify Apache Installation Open your web browser and navigate to your
EC2 instance\'s public IP address. You should see the Apache default
page.

4\. Persist the Mounts Across Reboots 4.1 Edit /etc/fstab bash

Verify

Open In Editor Edit Copy code sudo nano /etc/fstab Add the following
lines to mount the partitions automatically on reboot:

bash

Verify

Open In Editor Edit Copy code /dev/xvdf1 /mnt/vol1 ext4 defaults,nofail
0 2 /dev/xvdf2 /mnt/vol2 ext4 defaults,nofail 0 2 /dev/xvdf3 /mnt/vol3
ext4 defaults,nofail 0 2 4.2 Test the Configuration bash

Verify

Open In Editor Edit Copy code sudo mount -a 5. Detach the Volume from
the Current Instance 5.1 Safely Unmount the Volume bash

Verify

Open In Editor Edit Copy code sudo umount /mnt/vol1 sudo umount
/mnt/vol2 sudo umount /mnt/vol3 5.2 Detach the Volume via AWS Management
Console Go to the AWS Management Console. Navigate to the EC2 Dashboard.
Select \"Volumes\" from the left-hand menu. Find the volume you wish to
detach. Click on \"Actions\" \> \"Detach Volume\". Confirm the
detachment. 6. Attach the Volume to a New EC2 Instance 6.1 Attach the
Volume via AWS Management Console In the EC2 Dashboard, under
\"Volumes\", find the detached volume. Click on \"Actions\" \> \"Attach
Volume\". Select the new EC2 instance from the dropdown menu. Specify
the device name (
