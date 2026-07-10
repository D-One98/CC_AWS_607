# AWS Service Setup Guide: CloudTrail

## 1. Overview
CloudTrail records every API call made in the AWS account — who did what, from where, and when. It is the audit log of AWS. During the internship a multi-region CloudTrail was created that delivers events to an S3 bucket, and specific management events (e.g., `RunInstances`, `StopInstances`, IAM user creation) were searched in **Event history** to verify auditing.

## 2. Step-by-Step Setup Guide
1. Open the **CloudTrail** console → **Trails → Create trail**.
2. Trail name: `vineet-trail`. Storage location: **Create new S3 bucket** `vineet-cloudtrail-logs-<random>`. Log file SSE-KMS: default (or disable for lab simplicity).
3. Enable **Log file validation** (recommended). CloudWatch Logs: leave off for the basic lab.
4. Events: **Management events**, both **Read** and **Write**. **Data events** and **Insights**: off (they cost more).
5. Multi-region trail: **Yes**. Include global service events: **Yes**. Create trail.
6. Generate some activity: stop and start the EC2 instance, create a new IAM user, delete a security group rule.
7. Open **CloudTrail → Event history** and filter Event name = `RunInstances` or `StopInstances` — the recent actions should appear with the source IAM user and timestamp.
8. Open the S3 bucket `vineet-cloudtrail-logs-...` and drill down into the `AWSLogs/<account>/CloudTrail/<region>/<year>/<month>/<day>/` folder — JSON log files should be present.

## 3. Key Configurations Used
* **Trail name:** `vineet-trail`
* **Scope:** Multi-region + global service events
* **Log destination:** S3 bucket `vineet-cloudtrail-logs-...` (log file validation enabled)
* **Events captured:** Management events (Read + Write)
* **Data events / Insights:** Disabled (cost control)

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`cloudtrail_config.png`** — The **Trails** page showing `vineet-trail` with status **Logging**, multi-region = Yes, and the S3 destination bucket visible.
* **`cloudtrail_active.png`** — The **Event history** page filtered to a specific event such as `StopInstances` or `CreateUser`, showing the recent row with your IAM user as the source. Alternatively, an S3 screenshot of the JSON log files inside the CloudTrail bucket path.

*(Optional video: perform a Stop instance action, then jump to Event history and refresh to show the event appearing within 5 minutes.)*
