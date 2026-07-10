# AWS Service Setup Guide: CodePipeline

## 1. Overview
CodePipeline is AWS's managed CI/CD orchestrator — it chains a **Source** (GitHub / CodeCommit / S3), a **Build** (CodeBuild), and a **Deploy** (Elastic Beanstalk / CodeDeploy / ECS / S3) stage into an automated pipeline that runs on every commit. During the internship a pipeline was created that pulls from a GitHub repo on every push, builds the app with CodeBuild, and deploys to the Elastic Beanstalk environment from the previous lab.

## 2. Step-by-Step Setup Guide
1. Push the sample Beanstalk app to a GitHub repo `vineet-beanstalk-app` on the `main` branch. Include a **buildspec.yml** at the repo root:
   ```yaml
   version: 0.2
   phases:
     install:
       runtime-versions:
         python: 3.11
     build:
       commands:
         - echo Building...
   artifacts:
     files: ['**/*']
   ```
2. Open **CodePipeline** console → **Create pipeline**.
3. Pipeline name: `vineet-pipeline`. Service role: **New service role** (default name is fine). Create.
4. **Source stage**: Provider = **GitHub (Version 2)**. Connect to GitHub (create a new connection, authorize AWS). Repo: `vineet-beanstalk-app`. Branch: `main`. Detection: **AWS CodeStar source connection (webhook)**.
5. **Build stage**: Provider = **AWS CodeBuild**. Create new project `vineet-build`, environment = Managed image, Ubuntu, standard runtime latest, service role = auto-create. Buildspec: use the one in the repo.
6. **Deploy stage**: Provider = **AWS Elastic Beanstalk**. Application: `vineet-beanstalk`. Environment: `Vineet-beanstalk-env`.
7. Review and **Create pipeline**. It runs immediately.
8. When all three stages go green (Source → Build → Deploy), open the Beanstalk URL to confirm the new code is live.
9. Push a small change to `main` on GitHub — the pipeline should re-run automatically within a minute.

## 3. Key Configurations Used
* **Pipeline:** `vineet-pipeline`, artifact store S3 bucket auto-created
* **Source:** GitHub v2 connection → `vineet-beanstalk-app`, branch `main`
* **Build:** CodeBuild project `vineet-build`, Ubuntu managed image, `buildspec.yml`
* **Deploy:** Elastic Beanstalk app `vineet-beanstalk` / env `Vineet-beanstalk-env`
* **Trigger:** Webhook on every push to `main`

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`codepipeline_config.png`** — The **pipeline detail page** with all three stages (Source, Build, Deploy) shown green / **Succeeded**, and the commit hash / build number visible.
* **`codepipeline_active.png`** — Two-part proof (either combined or as two screenshots): a **GitHub commit** on the `main` branch **and** the Beanstalk environment URL in a browser showing the change from that commit. This proves the end-to-end automation.

*(Optional video: push a commit to GitHub in one window and record the pipeline stages turning from blue → green in the CodePipeline console, ending with the Beanstalk page reflecting the new content.)*
