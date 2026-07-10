# AWS Service Setup Guide: EFS (Elastic File System)

## 1. Overview
EFS is a **shared, network-attached file system (NFS v4.1)** that can be mounted on multiple EC2 instances at the same time — unlike EBS, which attaches to a single instance. During the internship an EFS file system was created and mounted on two EC2 instances in different subnets to demonstrate a shared `/mnt/efs` directory where a file written from Instance A was instantly visible on Instance B.

## 2. Step-by-Step Setup Guide
1. Open the **EFS** console → **Create file system**.
2. Name: `Vineet-EFS`. VPC: `Vineet-VPC`. Click **Customize** to configure mount targets.
3. **Network access**: create a mount target in **each AZ** where an EC2 instance lives, and attach a security group `Vineet-EFS-SG` that allows **NFS (port 2049)** inbound from the EC2 security group.
4. Leave performance mode = **General Purpose**, throughput = **Bursting**. Create the file system.
5. SSH into EC2 #1 and install the helper + mount:
   ```
   sudo yum install -y amazon-efs-utils
   sudo mkdir /mnt/efs
   sudo mount -t efs -o tls fs-xxxxxxxx:/ /mnt/efs
   df -h
   ```
6. SSH into EC2 #2 and repeat the mount.
7. On EC2 #1: `echo "shared from A" | sudo tee /mnt/efs/shared.txt`.
8. On EC2 #2: `cat /mnt/efs/shared.txt` — the content should appear immediately.
9. (Optional) Add the mount to `/etc/fstab` so it survives reboot.

## 3. Key Configurations Used
* **File system name:** `Vineet-EFS`, file system ID `fs-xxxxxxxx`
* **Mount targets:** one per AZ used, each in the correct subnet
* **Security group:** `Vineet-EFS-SG` — inbound NFS (2049) from `Vineet-WebSG`
* **Performance mode:** General Purpose, **Throughput mode:** Bursting
* **Mount point on EC2:** `/mnt/efs`

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`efs_config.png`** — The EFS **File system detail page** showing lifecycle state **Available** and the list of mount targets (one per AZ) also **available**.
* **`efs_active.png`** — Two SSH terminals side-by-side (or one screenshot with both): terminal A after writing `shared.txt`, terminal B running `cat /mnt/efs/shared.txt` and showing the same content. This is the proof of shared storage.

*(Optional video: split-screen recording of writing on Instance A and immediately reading on Instance B.)*
