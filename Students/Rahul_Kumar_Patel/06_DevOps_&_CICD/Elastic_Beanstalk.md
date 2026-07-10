# AWS Service Setup Guide: Elastic Beanstalk

## 1. Overview
Elastic Beanstalk is a Platform-as-a-Service that deploys a web application by automatically provisioning EC2, ELB, ASG, security groups, and CloudWatch behind the scenes — you upload a zip/war and Beanstalk handles the rest. During the internship a simple Python/Flask (or Node.js) sample app was deployed to a Beanstalk environment and then a new version was uploaded to demonstrate rolling updates.

## 2. Step-by-Step Setup Guide
1. Prepare a minimal Python app locally in a folder `app/`:
   ```
   # application.py
   from flask import Flask
   application = Flask(__name__)
   @application.route("/")
   def hello():
       return "Hello from Vineet's Beanstalk v1"
   ```
   plus a `requirements.txt` containing just `flask`. Zip the folder → `app-v1.zip`.
2. Open the **Elastic Beanstalk** console → **Create application**.
3. Application name: `vineet-beanstalk`. Environment tier: **Web server environment**.
4. Platform: **Python 3.11** (latest). Application code: **Upload your code** → upload `app-v1.zip`.
5. Presets: **Single instance (Free tier)** (or Load balanced if practicing production).
6. Under **Service access**: create a new service role and instance profile (Beanstalk suggests default names — accept them).
7. Under **Networking**: pick `Vineet-VPC` and a public subnet, and enable public IP.
8. Create environment. Wait 5–10 minutes for state = **Health: OK**.
9. Open the **Environment URL** in a browser — "Hello from Vineet's Beanstalk v1" should render.
10. Edit `application.py` to say `v2`, re-zip as `app-v2.zip`, then in the console click **Upload and deploy** → choose `app-v2.zip`. Refresh the URL to see v2.

## 3. Key Configurations Used
* **Application / Environment:** `vineet-beanstalk` / `Vineet-beanstalk-env`
* **Platform:** Python 3.11 (managed platform)
* **Preset:** Single instance (Free tier)
* **VPC / Subnet:** `Vineet-VPC` / public subnet
* **Deployed versions:** v1 (initial) → v2 (rolling deploy)

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`elastic_beanstalk_config.png`** — The Beanstalk **environment dashboard** showing Health = **OK**, running version, the environment URL, and the platform version.
* **`elastic_beanstalk_active.png`** — A **browser window** at the environment URL rendering the "Hello from Vineet's Beanstalk **v2**" text after the second deployment (URL bar visible).

*(Optional video: deploy v2, then refresh the browser to show the text updating from v1 to v2, showcasing zero-downtime rolling update.)*
