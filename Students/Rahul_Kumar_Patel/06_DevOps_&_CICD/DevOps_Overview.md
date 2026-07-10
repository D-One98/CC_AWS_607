# AWS Service Setup Guide: DevOps Overview

## 1. Overview
This document is the summary chapter for the DevOps track — it explains how the individual services (Elastic Beanstalk, CodePipeline, GitHub Actions, plus CloudWatch and SNS from monitoring) come together to form a real CI/CD workflow. The internship goal was: **code committed on GitHub → automatically built → automatically deployed to AWS → automatically monitored**, with a rollback path when a deploy fails.

## 2. Step-by-Step Setup Guide
1. **Source control (GitHub):** every app lives in its own GitHub repo on the `main` branch; feature work happens on branches and merges via pull requests.
2. **CI (build & test):** a GitHub Actions workflow (or CodeBuild inside CodePipeline) runs on every push — installs dependencies, runs unit tests, and packages the artifact.
3. **CD (deploy):** the built artifact is deployed automatically —
   * Static sites → `aws s3 sync` to the S3 bucket behind CloudFront/Route 53.
   * Server apps → Elastic Beanstalk `Upload and deploy` (via CodePipeline's Beanstalk deploy stage).
4. **Infrastructure:** all resources (VPC, EC2, ALB, RDS, ASG, WAF) sit inside the `Vineet-VPC` designed in the networking labs — created once, reused by every deployment.
5. **Monitoring:** CloudWatch alarms watch CPU, ALB 5XX rate, and Lambda errors; alarms publish to the SNS topic `vineet-alerts` which emails the owner.
6. **Audit:** CloudTrail records every deploy-time API call (StartBuild, CreateApplicationVersion, UpdateEnvironment) so any bad deploy can be traced back to a user and timestamp.
7. **Rollback path:** in Beanstalk, `Application versions → Deploy previous version`; in CodePipeline, disable the failing stage and re-run from the previous artifact.
8. **Security guardrails:** IAM least-privilege for the CI/CD user (`github-deployer`), MFA on human users, WAF in front of the ALB.

## 3. Key Configurations Used
* **Source repos:** `vineet-beanstalk-app`, `vineet-static-site`
* **CI/CD tooling:** GitHub Actions **and** AWS CodePipeline (both demonstrated)
* **Deploy targets:** Elastic Beanstalk env `Vineet-beanstalk-env`, S3 bucket `vineet-static-site`
* **Monitoring:** CloudWatch alarm `Vineet-High-CPU` → SNS topic `vineet-alerts` (email)
* **Audit / security:** CloudTrail trail `vineet-trail`, WAF ACL `vineet-waf-acl`, IAM user `github-deployer` (least privilege)

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the end-to-end DevOps flow.

### Screenshots to capture
* **`devops_overview_config.png`** — A **hand-drawn or diagrams.net architecture diagram** exported as PNG showing GitHub → GitHub Actions/CodePipeline → (Beanstalk / S3) → CloudWatch → SNS, with the VPC boundary drawn around the AWS resources. This is the "one picture that summarizes the internship" slide.
* **`devops_overview_active.png`** — A **combined screenshot** (or a small collage) showing three things at once: (1) a recent green pipeline run in either GitHub Actions or CodePipeline, (2) the deployed app URL rendering the latest change, and (3) a CloudWatch alarm in **OK** state. Together these prove that build, deploy, and monitoring are all live simultaneously.

*(Optional video: a 60-second narrated screen recording that walks from a `git push` → workflow green → deployed URL → CloudWatch dashboard. This is the ideal "demo day" artifact.)*
