# AWS Service Setup Guide: VPC (Virtual Private Cloud)

## 1. Overview
A VPC is a logically isolated virtual network inside AWS where all your EC2 instances, RDS databases, and load balancers live. It defines the IP address range, subnets (public/private), routing, and internet connectivity. During the internship a custom VPC was built from scratch to understand CIDR planning, public vs. private subnet separation, and how the Internet Gateway + route tables together decide what traffic can leave or enter the network.

## 2. Step-by-Step Setup Guide
1. Open the **VPC** console and choose **Create VPC → VPC and more** (this creates the VPC, subnets, route tables, and gateway in one flow).
2. Name tag: `Vineet-VPC`. IPv4 CIDR block: `10.0.0.0/16`.
3. Choose **2 Availability Zones**, **2 public subnets** (`10.0.1.0/24`, `10.0.2.0/24`) and **2 private subnets** (`10.0.11.0/24`, `10.0.12.0/24`).
4. NAT gateways: **None** (for cost) or **In 1 AZ** if private subnets need outbound internet.
5. VPC endpoints: **None** for the basic lab.
6. Enable **DNS hostnames** and **DNS resolution**, then click **Create VPC**.
7. Verify that an **Internet Gateway** is attached and the **public route table** has a `0.0.0.0/0 → igw-...` route.
8. Verify the **private route table** has **no** default route to the IGW (this is what makes those subnets private).

## 3. Key Configurations Used
* **VPC CIDR:** `10.0.0.0/16`
* **Public subnets:** `10.0.1.0/24` (AZ-a), `10.0.2.0/24` (AZ-b)
* **Private subnets:** `10.0.11.0/24` (AZ-a), `10.0.12.0/24` (AZ-b)
* **Internet Gateway:** Attached to `Vineet-VPC`
* **Route tables:** Public RT (0.0.0.0/0 → IGW), Private RT (local only)
* **DNS hostnames / resolution:** Enabled

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`vpc_config.png`** — The **Resource map** view of the VPC (VPC → your VPC → Resource map tab) that visually shows the VPC, both AZs, public/private subnets, route tables, and the internet gateway all connected.
* **`vpc_active.png`** — The **Subnets** list filtered to `Vineet-VPC` showing all 4 subnets in the `Available` state with their CIDRs and AZs.

*(Optional video: a short recording panning across the Resource map or clicking into a route table to show the `0.0.0.0/0 → igw` entry.)*
