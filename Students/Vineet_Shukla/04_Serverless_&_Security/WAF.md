# AWS Service Setup Guide: WAF (Web Application Firewall)

## 1. Overview
AWS WAF filters HTTP(S) traffic before it reaches the application — blocking common attack patterns (SQL injection, XSS), rate-limiting abusive IPs, and enforcing geo or IP allow/deny lists. During the internship a Web ACL was created and attached to the ALB, protected with the **AWS Managed Rules – Common Rule Set**, and a **custom rate-based rule** was added to block any client sending more than 100 requests in 5 minutes.

## 2. Step-by-Step Setup Guide
1. Open the **WAF & Shield** console → **Web ACLs → Create web ACL**.
2. Name: `vineet-waf-acl`. Resource type: **Regional resources (ALB, API Gateway)**. Region: `ap-south-1`.
3. Associated AWS resources: **Add AWS resources** → Application Load Balancer → `vineet-alb`.
4. **Add rules and rule groups → Add managed rule groups → AWS managed rule groups**.
5. Enable **Core rule set (AWSManagedRulesCommonRuleSet)** and **Known bad inputs (AWSManagedRulesKnownBadInputsRuleSet)**. Save.
6. Add another rule → **Add my own rules and rule groups → Rule builder**. Name: `rate-limit-100`. Type: **Rate-based rule**. Rate limit: **100** requests per 5 minutes per IP. Action: **Block**. Save.
7. Default web ACL action: **Allow**. Set rule priorities (rate rule first is fine). Create.
8. Test blocking: from a terminal, hammer the ALB URL to trigger the rate limit:
   ```
   for i in $(seq 1 200); do curl -s -o /dev/null -w "%{http_code}\n" http://<alb-dns>/; done
   ```
   After about 100 requests responses should start returning **403 Forbidden**.
9. Open the WAF Web ACL → **Overview** tab to see the request/blocked graph.

## 3. Key Configurations Used
* **Web ACL name / scope:** `vineet-waf-acl` / Regional (ap-south-1)
* **Associated resource:** ALB `vineet-alb`
* **Managed rule groups:** Core rule set, Known bad inputs
* **Custom rule:** `rate-limit-100` — 100 req/5 min per IP, Block
* **Default action:** Allow

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`waf_config.png`** — The **Web ACL detail page** showing the associated ALB, the managed rule groups, and the custom rate-based rule with priority order visible.
* **`waf_active.png`** — The **Overview / Sampled requests** tab of the Web ACL showing a spike of **Blocked requests** (or a terminal screenshot showing curl responses flipping from `200` to `403` after the rate limit trips). This is the actual proof WAF is enforcing.

*(Optional video: run the curl loop and screen-record the Blocked-requests counter on the WAF Overview tab climbing in real time.)*
