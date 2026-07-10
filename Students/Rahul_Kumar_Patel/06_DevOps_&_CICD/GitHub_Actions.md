# AWS Service Setup Guide: GitHub Actions (deploying to AWS)

## 1. Overview
GitHub Actions is GitHub's built-in CI/CD system — workflows defined in YAML under `.github/workflows/` run on every push. It is a common alternative to CodePipeline for AWS deployments. During the internship a workflow was written that, on every push to `main`, authenticates to AWS using an IAM access key stored in GitHub Secrets and syncs the built site to the S3 static-website bucket from the S3 lab.

## 2. Step-by-Step Setup Guide
1. In AWS, create an IAM user `github-deployer` with a policy that only allows `s3:PutObject`, `s3:DeleteObject`, `s3:ListBucket` on the `vineet-static-site` bucket. Create an access key for this user.
2. In the GitHub repo `vineet-static-site`, go to **Settings → Secrets and variables → Actions → New repository secret** and add:
   * `AWS_ACCESS_KEY_ID`
   * `AWS_SECRET_ACCESS_KEY`
   * `AWS_REGION` = `ap-south-1`
   * `S3_BUCKET` = `vineet-static-site`
3. Create the workflow at `.github/workflows/deploy.yml`:
   ```yaml
   name: Deploy to S3
   on:
     push:
       branches: [ main ]
   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
         - name: Configure AWS credentials
           uses: aws-actions/configure-aws-credentials@v4
           with:
             aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
             aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
             aws-region: ${{ secrets.AWS_REGION }}
         - name: Sync to S3
           run: aws s3 sync ./site s3://${{ secrets.S3_BUCKET }} --delete
   ```
4. Commit and push. On GitHub open the **Actions** tab — the workflow should start and go green.
5. Open the S3 static website URL — the new file(s) should be live.
6. Edit `site/index.html`, push again, and watch the workflow re-run and the website update.

## 3. Key Configurations Used
* **Repo:** `vineet-static-site` on GitHub
* **Workflow file:** `.github/workflows/deploy.yml`, trigger = push to `main`
* **AWS IAM user:** `github-deployer` (least-privilege on the S3 bucket only)
* **GitHub Secrets:** `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`, `S3_BUCKET`
* **Deploy command:** `aws s3 sync ./site s3://<bucket> --delete`

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`github_actions_config.png`** — The **Actions tab** of the GitHub repo showing the "Deploy to S3" workflow run with all steps green / **Success**, and the commit that triggered it visible.
* **`github_actions_active.png`** — The **S3 static website URL** open in a browser rendering the change from that commit. Include the URL bar so the endpoint is visible.

*(Optional video: edit `index.html` locally, `git push`, then split-screen the GitHub Actions run turning green while the S3 site refreshes to show the new content.)*
