# AWS Service Setup Guide: Route 53

## 1. Overview
Route 53 is AWS's DNS service. It maps human-friendly domain names (e.g., `www.example.com`) to AWS resources like ALBs, EC2 IPs, or S3 websites. During the internship a **hosted zone** was created (using either a real registered domain or a test subdomain), and DNS records were configured to point to the ALB from the ELB lab so the site could be reached by name instead of by the ALB's long AWS DNS.

## 2. Step-by-Step Setup Guide
1. Open **Route 53 → Hosted zones → Create hosted zone**.
2. Domain name: `vineetshukla-lab.com` (or your own registered domain). Type: **Public hosted zone**. Create.
3. Note the four **NS (name-server)** records shown at the top of the hosted zone. If the domain is registered outside AWS, update the domain's registrar to use these four name servers.
4. Inside the hosted zone → **Create record**.
5. Record 1: name `www`, type **A**, toggle **Alias: Yes**, route traffic to **Application Load Balancer**, region, then pick `vineet-alb`. Create.
6. Record 2: name (leave blank for apex), type **A**, Alias → same ALB. Create.
7. Wait a few minutes for DNS propagation.
8. From a terminal: `dig www.vineetshukla-lab.com +short` — should return the ALB's IPs.
9. Open `http://www.vineetshukla-lab.com` in a browser — the ALB's response page should load.

## 3. Key Configurations Used
* **Hosted zone:** `vineetshukla-lab.com` (public)
* **Records:** `www` (A / Alias → ALB), apex (A / Alias → ALB)
* **TTL:** default (Alias records manage TTL automatically)
* **Routing policy:** Simple
* **Name servers:** the 4 AWS NS records assigned to the hosted zone

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`route53_config.png`** — The **Records** view of the hosted zone showing the `www` and apex A/Alias records with the ALB as the target.
* **`route53_active.png`** — A **browser window** at `http://www.vineetshukla-lab.com` (the URL bar must be visible) rendering the ALB's Hello page. Optionally include a terminal `dig` / `nslookup` output next to it.

*(Optional video: run `dig www.<domain> +short` first, then open the URL in a browser to show name → ALB → page in one flow.)*
