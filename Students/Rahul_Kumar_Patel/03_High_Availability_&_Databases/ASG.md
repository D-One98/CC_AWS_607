# AWS Service Setup Guide: ASG (Auto Scaling Group)

## 1. Overview
An Auto Scaling Group automatically launches or terminates EC2 instances to maintain a desired count and to scale in/out based on CPU or other CloudWatch metrics. During the internship an ASG was created behind the ALB from the previous lab, with min=1, desired=2, max=4. A CPU stress test was then run on one instance to trigger a scale-out event, and stopping the load caused the group to scale back in.

## 2. Step-by-Step Setup Guide
1. Open **EC2 → Launch Templates → Create launch template**.
2. Name: `vineet-lt`. AMI: Amazon Linux 2023. Instance type: t2.micro. Key pair: `vineet-key`. Security group: `Vineet-WebSG`. User data: the same Apache install script from the EC2 doc.
3. Create launch template.
4. Open **EC2 → Auto Scaling Groups → Create Auto Scaling group**.
5. Name: `vineet-asg`. Launch template: `vineet-lt` (latest version).
6. VPC: `Vineet-VPC`. AZs & subnets: **both public subnets**.
7. Load balancing: **Attach to an existing load balancer** → target group `vineet-tg` (from the ELB lab).
8. Health checks: enable **ELB** health checks. Grace period: 60 s.
9. Group size: **Desired = 2, Min = 1, Max = 4**.
10. Scaling policy: **Target tracking**, metric = **Average CPU utilization**, target value = **50%**.
11. Create the ASG. Two instances launch automatically and register with the target group.
12. SSH into one instance and run a stress test to force scale-out:
    ```
    sudo yum install -y stress
    stress --cpu 2 --timeout 300
    ```
13. Watch **Activity → Activity history** on the ASG: a scale-out event should appear, and a new instance joins the target group.

## 3. Key Configurations Used
* **Launch template:** `vineet-lt` (AL2023, t2.micro, `Vineet-WebSG`, Apache user data)
* **ASG name:** `vineet-asg` — Min 1 / Desired 2 / Max 4
* **Subnets:** Both public subnets across 2 AZs
* **Load balancer attach:** target group `vineet-tg`
* **Scaling policy:** Target tracking, avg CPU = 50%
* **Health check type:** ELB, grace period 60 s

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`asg_config.png`** — The **ASG detail page** showing Min/Desired/Max, the attached target group, both AZs, and the current instance count.
* **`asg_active.png`** — The **Activity history** tab of the ASG showing a **"Launching a new EC2 instance"** event caused by the CPU scale-out, or the **Instance management** tab showing 3+ instances after stress load. This is what proves auto scaling actually fired.

*(Optional video: run the stress command in one terminal while the ASG Activity page is open, and record the scale-out event appearing live.)*
