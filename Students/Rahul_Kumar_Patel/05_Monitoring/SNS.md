# AWS Service Setup Guide: SNS (Simple Notification Service)

## 1. Overview
SNS is a pub/sub messaging service — publishers push messages to a **Topic**, and any number of **Subscribers** (email, SMS, Lambda, SQS, HTTPS endpoints) receive them. During the internship an SNS topic `vineet-alerts` was created with an email subscription and used as the notification target for the CloudWatch CPU alarm.

## 2. Step-by-Step Setup Guide
1. Open the **SNS** console → **Topics → Create topic**.
2. Type: **Standard**. Name: `vineet-alerts`. Display name: `Vineet Alerts`. Create.
3. Open the topic → **Create subscription**. Protocol: **Email**. Endpoint: your email address. Create.
4. Open the confirmation email from AWS and click **Confirm subscription**. The subscription status in the console should flip to **Confirmed**.
5. From the topic page click **Publish message**. Subject: `Test`. Message body: `Hello from Vineet's SNS topic`. **Publish**.
6. Check the inbox — the email should arrive within seconds.
7. (Optional) Add a second subscription with protocol **SMS** and your phone number to test text alerts.
8. Wire the topic into the CloudWatch alarm from the CloudWatch lab (already done there) so real alarms fire notifications through this topic.

## 3. Key Configurations Used
* **Topic name / type:** `vineet-alerts` / Standard
* **Subscription 1:** Email — status **Confirmed**
* **Optional Subscription 2:** SMS — phone number confirmed
* **Publisher(s):** Manual "Publish message" test + CloudWatch alarm `Vineet-High-CPU`

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`sns_config.png`** — The **SNS topic detail page** for `vineet-alerts` showing the topic ARN and the subscriptions list with status **Confirmed**.
* **`sns_active.png`** — A screenshot of the **received email** in the inbox (from AWS Notifications, subject `Test` or the CloudWatch alarm subject). This is the proof end-to-end delivery works.

*(Optional video: click **Publish message** in the console and immediately show the email arriving in the inbox.)*
