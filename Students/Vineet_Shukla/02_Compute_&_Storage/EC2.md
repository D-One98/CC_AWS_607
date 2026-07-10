# AWS Service Setup Guide: EC2 (Elastic Compute Cloud)

## 1. Overview
EC2 provides resizable virtual machines ("instances") in the cloud. During the internship EC2 was the workhorse — used to host a simple web server (Apache/Nginx), to test SSH access, to attach EBS volumes and EFS, and later to sit behind an ELB inside an Auto Scaling Group. It is the first service every AWS learner should understand deeply.

## 2. Step-by-Step Setup Guide
1. Open the **EC2** console → **Instances → Launch instances**.
2. Name: `Vineet-WebServer`. AMI: **Amazon Linux 2023** (Free tier eligible).
3. Instance type: **t2.micro** or **t3.micro** (Free tier).
4. Key pair: create a new key pair `vineet-key`, type **RSA**, format **.pem**, and download it (keep it safe — it cannot be re-downloaded).
5. Network settings → **Edit**: choose `Vineet-VPC`, a **public subnet**, enable **Auto-assign public IP**.
6. Create a new **security group** `Vineet-WebSG` with inbound rules: **SSH (22)** from *My IP*, **HTTP (80)** from *Anywhere*.
7. Storage: leave the default 8 GiB gp3 root volume.
8. Under **Advanced → User data**, paste the Apache install script:
   ```
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<h1>Hello from Vineet's EC2</h1>" > /var/www/html/index.html
   ```
9. Click **Launch instance**. Wait for **Instance state = Running** and **Status checks = 2/2**.
10. Copy the **Public IPv4 DNS** and open it in a browser — the "Hello" page should load.
11. SSH test: `ssh -i vineet-key.pem ec2-user@<public-ip>`.

## 3. Key Configurations Used
* **AMI:** Amazon Linux 2023
* **Instance type:** t2.micro (Free tier)
* **Key pair:** `vineet-key.pem`
* **VPC / Subnet:** `Vineet-VPC` / public subnet
* **Security group:** `Vineet-WebSG` (SSH 22 from My IP, HTTP 80 from 0.0.0.0/0)
* **User data:** Apache install + custom index.html

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
<video controls src="../Screenshort/Screen Recording 2026-07-06 145322.mp4" title="Title"></video>