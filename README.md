# Amazon EBS Volume Creation, Partitioning, and Mounting on an Amazon EC2 Instance



## Introduction

This project demonstrates how to create, attach, partition, format, and mount an Amazon Elastic Block Store (EBS) volume on an Amazon EC2 Linux instance. The project covers the complete storage management process, including verifying the attached disk, creating a file system, mounting the volume, and validating the successful configuration using Linux commands.



## Architecture Diagram

![](./images/architecture%20diagram.jpeg)

### Architecture Workflow
1. **AWS Console:** Created a 5 GiB gp3 EBS volume in Availability Zone `us-east-1a`.
2. **Attachment:** Attached the EBS volume to the running EC2 instance as device `/dev/sdb`.
3. **SSH Access:** Connected to the EC2 Linux instance via SSH terminal.
4. **Linux Disk Management:** Identified the raw disk (`/dev/nvme1n1`), created a partition (`/dev/nvme1n1p1`), formatted it with XFS (`mkfs.xfs`), created mount points (`/opt/old-disk` or `/mnt/old-disk`), and mounted the file system successfully.



## Implementation Steps

### Step 1: Check Existing Storage
* Executed the `lsblk` command on the Linux terminal to view the attached block devices.
* Identified the primary root volume (e.g., `nvme0n1`) and its existing system partitions.
* Confirmed the available storage capacity and partition layout prior to attaching new volumes.
* Ensured no unmounted secondary disk was present before proceeding with volume attachment.

![](./images/check%20existing%20storage.png)


### Step 2: Create Amazon EBS Volume
* Navigated to the AWS EC2 Management Console under the **Volumes** section.
* Specified the volume configuration parameters including size (5 GiB) and volume type (gp3).
* Selected the exact Availability Zone (`us-east-1a`) where the target EC2 instance was running.
* Finalized the creation process and verified that the volume state changed to `available`.

![](./images/create%20volume%201.png)
![](./images/create%20volume%202.png)
![](./images/create%20volume%203.png)
![](./images/create%20volume.png)


### Step 3: Attach Amazon EBS Volume
* Selected the newly created EBS volume from the AWS Console and clicked **Actions -> Attach volume**.
* Selected the target running EC2 instance located in the matching Availability Zone.
* Assigned the device name `/dev/sdb` for the attachment mapping.
* Verified in the console that the EBS volume status transitioned to `In-use`.

![](./images/attach%20volume%201.png)
![](./images/attach%20volume%202.png)
![](./images/attach%20volume.png)


### Step 4: Verify Attached EBS Volume
* Re-ran the `lsblk` or `fdisk -l` command on the EC2 terminal to confirm detection.
* Verified that the Linux kernel detected the new raw block device (e.g., `nvme1n1`).
* Checked the disk size (5 GiB) to ensure proper mapping with the attached EBS volume.
* Confirmed that the new device currently lacks a file system and partition table.

![](./images/verify%20attached%20EBS%20volume.png)


### Step 5: Create a New Partition
* Initiated the disk partitioning utility using the command `sudo fdisk /dev/nvme1n1`.
* Created a new primary partition using the `n` command in the interactive prompt.
* Selected default partition parameters for first/last sectors to allocate storage.
* Wrote the partition table changes to the disk using the `w` command in `fdisk`.

![](./images/Create%20a%20new%20partition1.png)
![](./images/create%20a%20new%20partition2.png)


### Step 6: Verify Created Partitions
* Executed `lsblk` to confirm the successful creation of the partition.
* Verified the generation of the partition device node (e.g., `/dev/nvme1n1p1`).
* Checked the allocated size of the partition against the main volume size.
* Ensured the partition was ready to be formatted with a Linux file system.

![](./images/verify%20created%20partitions.png)


### Step 7: Create File System
* Executed `sudo mkfs.xfs /dev/nvme1n1p1` to format the newly created partition.
* Formatted the partition using the XFS file system to support scalable storage management.
* Verified block allocation, metadata structural integrity, and inode configuration.
* Ensured the block device was properly prepared for directory mounting.

![](./images/create%20file%20system.png)


### Step 8: Create Mount Directories
* Navigated to standard system mount points such as `/opt` and `/mnt`.
* Executed `sudo mkdir old-disk` to create a mount point directory.
* Used `ls` command to verify successful directory creation in the target path.
* Confirmed appropriate directory permissions for attaching the file system.

![](./images/create%20mount%20directory1.png)
![](./images/create%20mount%20directory2.png)

### Step 9: Mount File Systems
* Associated the XFS partition with the mount directory using `sudo mount /dev/nvme1n1p1 /opt/old-disk`.
* Linked the formatted raw storage to the Linux directory tree structure.
* Verified write and read accessibility on the newly mounted file system.
* Ensured no mounting conflicts or device busy errors occurred during execution.

![](./images/mount%20file%20system.png)


### Step 10: Verification
* Ran `lsblk` and `df -h` commands to verify active mount points and disk usage.
* Confirmed that `/dev/nvme1n1p1` was successfully mapped to the target directory.
* Inspected total capacity, used space, and available storage on the mounted mountpoint.
* Validated that the EBS volume lifecycle configuration was fully functional.



## Project Summary

This project demonstrates the complete lifecycle of managing Amazon EBS storage on an Amazon EC2 instance. It includes creating an EBS volume, attaching it to an EC2 instance, creating partitions, formatting them with the XFS file system, creating mount directories, mounting the file systems, and verifying the successful configuration. This project provides practical knowledge of AWS storage services and Linux disk management, making it an essential hands-on exercise for AWS and DevOps learners.

