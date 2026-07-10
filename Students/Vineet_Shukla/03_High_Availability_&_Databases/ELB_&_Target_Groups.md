# AWS Service Setup Guide: ELB & Target Groups

## 1. Overview
An **Elastic Load Balancer** distributes incoming traffic across multiple EC2 instances for high availability and horizontal scaling. A **Target Group** is the pool of instances (or IPs / Lambdas) that the load balancer forwards traffic to and continuously health-checks. During the internship an **Application Load Balancer (ALB)** was placed in front of two EC2 web servers in two AZs to demonstrate traffic distribution and failover when one instance was stopped.

## 2. Step-by-Step Setup Guide
1. Launch **two EC2 instances** in two different AZs (both running the Apache "Hello" server from the EC2 doc), each with a different index.html so you can tell which one served the request (e.g., "Server-A" and "Server-B").
2. Open **EC2 → Target Groups → Create target group**.
3. Target type: **Instances**. Name: `vineet-tg`. Protocol: **HTTP**, Port: **80**. VPC: `Vineet-VPC`. Health check path: `/`.
4. Register both EC2 instances as targets on port 80. **Create target group**.
5. Open **EC2 → Load Balancers → Create load balancer → Application Load Balancer**.
6. Name: `vineet-alb`. Scheme: **Internet-facing**. IP type: IPv4.
7. VPC: `Vineet-VPC`. Mappings: select **both public subnets** (both AZs).
8. Security group: create/select one allowing **HTTP (80)** from `0.0.0.0/0`.
9. Listener: HTTP :80 → forward to `vineet-tg`. Create load balancer.
10. Wait for state = **Active**. Copy the **DNS name** and refresh it in a browser multiple times — responses should alternate between Server-A and Server-B.
11. Stop one EC2 instance and observe the target group marking it **unhealthy**; the ALB automatically routes only to the remaining healthy instance.

## 3. Key Configurations Used
* **LB type:** Application Load Balancer, internet-facing
* **Listener:** HTTP :80 → target group `vineet-tg`
* **Target group:** HTTP :80, health check path `/`, threshold 2/2
* **Targets:** 2 EC2 instances across 2 AZs
* **Subnets:** Both public subnets of `Vineet-VPC`
* **Security group:** Inbound HTTP 80 from 0.0.0.0/0

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`elb_target_groups_config.png`** — The **Target group** page showing both targets with health status **healthy** on port 80, and (in the same shot or as a second screenshot) the ALB detail page showing state **Active** with its DNS name visible.
* **`elb_target_groups_active.png`** — Two side-by-side **browser tabs** loaded at the ALB DNS name showing "Server-A" and "Server-B" respectively (refresh a few times until both appear), proving the ALB is distributing traffic.

*(Optional video: refresh the ALB URL a few times to visibly alternate between servers, then stop one instance and refresh again to show automatic failover.)*
