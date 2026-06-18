# Linux for DevOps – Attaching EBS Volume to EC2

Step-by-step tutorial on creating, attaching, formatting, and mounting EBS volumes in EC2 instances.

## Access the walkthrough

[![Linux EBS Volume Tutorial](https://img.youtube.com/vi/--2ZVkishcI/hqdefault.jpg)](https://www.youtube.com/embed/--2ZVkishcI?si=uJVCgEhr3LdhKw3p)

[Watch the YouTube Video](https://www.youtube.com/embed/--2ZVkishcI?si=uJVCgEhr3LdhKw3p)
---

## 🛠 Step-by-Step Strategy

### Step 1: Create an EC2 Instance
Launch a free-tier EC2 instance in the default VPC. Ensure the security group allows SSH access (port 22).

<div align="center">
  <img src="images/linux-ec2-volume/Step 1 Create EC2 Instance/ec2_create1.png" width="250"/>
  <!-- continue up to ec2_create15.png -->
  <img src="images/linux-ec2-volume/Step 1 Create EC2 Instance/ec2_create15.png" width="250"/>
</div>

---

### Step 2: Connect to Instance via SSH
Use your PEM key to connect:

bash
ssh -i "C:\Users\DELL\Downloads\kaif-devops-demo.pem" ubuntu@15.206.66.138
<div align="center">
<img src="images/linux-ec2-volume/Step 2 SSH Connect/ssh_connect1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 2 SSH Connect/ssh_connect5.png" width="300"/>
</div>

### Step 3: Inspect Default Disk
Check existing block devices and partitions:

bash
lsblk
df -h /
ls -ltr /boot
sudo fdisk -l
<div align="center">
<img src="images/linux-ec2-volume/Step 3 Understanding the existing default disk and commands/step3_unederstand_default1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 3 Understanding the existing default disk and commands/step3_unederstand_default5.png" width="300"/>
</div>

### Step 4.1: Create EBS Volume
In the AWS console, go to Elastic Block Store (EBS) → Volumes. Create a new 9GB volume in the same AZ as your EC2 instance.

<div align="center">
<img src="images/linux-ec2-volume/Step 4.1 Create Volume from EBS/Step4.1_Create_Volume1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 4.1 Create Volume from EBS/Step4.1_Create_Volume7.png" width="300"/>
</div>

### Step 4.2: Attach Volume to Instance
Select the created volume → Actions → Attach Volume. Choose your EC2 instance and set the device name (e.g., /dev/sdf).

<div align="center">
<img src="images/linux-ec2-volume/Step 4.2 Attach Created Volume to instance/Step4.2_Attach_Volume1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 4.2 Attach Created Volume to instance/Step4.2_Attach_Volume5.png" width="300"/>
</div>

### Step 5: Create Mount Directory
Switch to root and create a mount directory:

bash
sudo su -
mkdir -p /mnt/kaif-volume
ls -ltr /mnt
<div align="center">
<img src="images/linux-ec2-volume/Step 5 Make Mount Directory to attach that volume/step5_make_mount_dir1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 5 Make Mount Directory to attach that volume/step5_make_mount_dir3.png" width="300"/>
</div>

### Step 6.1: Format the Volume
Format the new block device (xvdf) with ext4:

bash
mkfs -t ext4 /dev/xvdf
<div align="center">
<img src="images/linux-ec2-volume/Step 6.1 Make File System or Format the Block to ext4/step6.1_make_filesystem.png" width="400"/>
</div>

### Step 6.2: Mount the Volume
Mount the formatted volume to the directory:

bash
mount /dev/xvdf /mnt/kaif-volume/
<div align="center">
<img src="images/linux-ec2-volume/Step 6.2 Mount the file system into the created directory/step6.2_mount_filesystem1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 6.2 Mount the file system into the created directory/step6.2_mount_filesystem2.png" width="300"/>
</div>

### Step 7: Verify by Creating a File
Navigate into the mounted directory and create a test file:

bash
cd /mnt/kaif-volume
touch kaifkhan
ls
df -h
<div align="center">
<img src="images/linux-ec2-volume/Step 7 Create file inside it to confirm its working/step7_confirm_working1.png" width="300"/>
<img src="images/linux-ec2-volume/Step 7 Create file inside it to confirm its working/step7_confirm_working2.png" width="300"/>
</div>

---

### 📝 Notes

The default root disk (xvda) contains partitions for root (~7GB), boot (~1GB), and system tasks.

When creating a new EBS volume, ensure the availability zone matches your EC2 instance (e.g., ap-south-1a).

After attaching the new volume (xvdf), format it with a filesystem (e.g., ext4) and mount it to a directory (e.g., /mnt/kaif-volume) to make it usable.
