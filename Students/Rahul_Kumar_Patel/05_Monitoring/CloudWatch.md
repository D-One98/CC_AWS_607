# AWS Service Setup Guide: CloudWatch

## 1. Overview
CloudWatch collects metrics, logs, and events from AWS services and lets you build dashboards, alarms, and log searches. During the internship CloudWatch was used to view EC2 CPU metrics, create a **CPU > 70%** alarm on the web-server instance that notified an SNS topic, and to inspect Lambda execution logs under `/aws/lambda/*` log groups.

## 2. Step-by-Step Setup Guide
1. Open the **CloudWatch** console → **Metrics → All metrics → EC2 → Per-Instance Metrics** and select the web server's `CPUUtilization` — confirm data points are flowing.
2. Go to **Alarms → All alarms → Create alarm**.
3. **Select metric** → EC2 → Per-Instance → `CPUUtilization` for `Vineet-WebServer` instance ID.
4. Statistic: **Average**, Period: **1 minute**.
5. Condition: **Static**, greater than **70**. Datapoints to alarm: **2 out of 2**.
6. Notification: **In alarm → Create new SNS topic** `vineet-alerts`, enter your email, **Create topic** (confirm the email you receive).
7. Name the alarm `Vineet-High-CPU`. Create.
8. Trigger it: SSH into the EC2 and run `stress --cpu 2 --timeout 300`. Watch the alarm transition **OK → In alarm**, and check the email inbox for the SNS notification.
9. Open **Logs → Log groups → /aws/lambda/vineet-hello** to view the Lambda execution logs from the Lambda lab.
10. (Optional) Build a **Dashboard**: **Dashboards → Create dashboard → `Vineet-Ops`** and add widgets for EC2 CPU, ALB request count, and Lambda invocations.

## 3. Key Configurations Used
* **Alarm name:** `Vineet-High-CPU`
* **Metric:** `AWS/EC2 CPUUtilization`, Average, 1-min period
* **Threshold:** > 70% for 2 consecutive periods
* **Notification target:** SNS topic `vineet-alerts` → email subscribed
* **Log groups reviewed:** `/aws/lambda/vineet-hello`
* **Dashboard (optional):** `Vineet-Ops` with EC2 / ALB / Lambda widgets

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`cloudwatch_config.png`** — The **Alarms** page showing `Vineet-High-CPU` with its threshold, associated instance, and SNS action. A screenshot of the alarm detail (graph + threshold line) is even better.
* **`cloudwatch_active.png`** — The alarm in **In alarm** state during the stress test (red banner on the alarm detail page) **plus** the email notification received from `vineet-alerts` — this pair proves the full metric → alarm → notification chain works.

*(Optional video: split-screen the alarm detail page and the terminal running `stress`, and record the state changing from OK to In alarm along with the email popping up.)*
