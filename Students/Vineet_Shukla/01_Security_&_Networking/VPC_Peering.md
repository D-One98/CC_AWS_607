# AWS Service Setup Guide: VPC Peering

## 1. Overview
VPC Peering creates a private, one-to-one network connection between two VPCs so that resources in each can communicate using private IPs as if they were in the same network — without going over the public internet. During the internship a peering connection was set up between two custom VPCs to demonstrate cross-VPC EC2-to-EC2 private communication (ping over private IP after route + security group updates).

## 2. Step-by-Step Setup Guide
1. Create a second VPC (e.g., `Vineet-VPC-B` with CIDR `10.1.0.0/16`) so the two VPCs have **non-overlapping** CIDRs.
2. Open **VPC → Peering connections → Create peering connection**.
3. Name: `Vineet-Peering`. Requester VPC: `Vineet-VPC` (10.0.0.0/16). Accepter VPC: `Vineet-VPC-B` (10.1.0.0/16). Same account, same region.
4. Click **Create peering connection**, then select it and choose **Actions → Accept request**.
5. Open the **route table of VPC A** and add a route: destination `10.1.0.0/16`, target `pcx-...` (the peering connection).
6. Open the **route table of VPC B** and add a route: destination `10.0.0.0/16`, target the same `pcx-...`.
7. Update the **security groups** on the test EC2 instances to allow ICMP/SSH from the other VPC's CIDR.
8. Launch a test EC2 in each VPC and `ping <private-IP-of-other-instance>` to verify.

## 3. Key Configurations Used
* **Peering name:** `Vineet-Peering`
* **VPC A CIDR:** `10.0.0.0/16` — **VPC B CIDR:** `10.1.0.0/16` (non-overlapping)
* **Peering status:** Active (after acceptance)
* **Routes added:** A → B via `pcx-...`, B → A via `pcx-...`
* **Security groups:** ICMP allowed from the peer CIDR for the ping test

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`vpc_peering_config.png`** — The **Peering connections** page showing `Vineet-Peering` with status **Active**, and the requester/accepter VPC IDs + CIDRs visible.
* **`vpc_peering_active.png`** — A **terminal / SSH window on the EC2 in VPC A** successfully running `ping <private-IP-of-EC2-in-VPC-B>` with replies coming back. This is the proof that the peering actually works end-to-end.

*(Optional video: 30-second recording of running the ping from one instance to the other and showing the route table entry with the `pcx-...` target.)*
