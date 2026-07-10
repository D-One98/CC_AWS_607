# AWS Service Setup Guide: Static Website Hosting (S3)

## 1. Overview
S3 can serve static websites (HTML, CSS, JS, images) directly from a bucket without any EC2 server. During the internship a simple `index.html` + `error.html` pair was deployed to an S3 bucket, the bucket was made public, static hosting was enabled, and the resulting website endpoint was opened in a browser to confirm the page rendered.

## 2. Step-by-Step Setup Guide
1. Create a new bucket `vineet-static-site` (must be globally unique). Region: `ap-south-1`.
2. On the create page, **uncheck "Block all public access"** and acknowledge the warning — this bucket must be public.
3. Create the bucket, then upload two files: `index.html` (with any content) and `error.html`.
4. Open the bucket → **Properties → Static website hosting → Edit**.
5. **Enable** static website hosting. Hosting type: **Host a static website**. Index document: `index.html`. Error document: `error.html`. **Save changes**.
6. Note the **Bucket website endpoint** URL that now appears (e.g., `http://vineet-static-site.s3-website.ap-south-1.amazonaws.com`).
7. Open the bucket → **Permissions → Bucket policy → Edit** and add:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Sid": "PublicReadGetObject",
       "Effect": "Allow",
       "Principal": "*",
       "Action": "s3:GetObject",
       "Resource": "arn:aws:s3:::vineet-static-site/*"
     }]
   }
   ```
8. Open the website endpoint URL in a browser — `index.html` should render. Visiting a non-existent path should render `error.html`.

## 3. Key Configurations Used
* **Bucket name:** `vineet-static-site`
* **Block Public Access:** OFF (public bucket)
* **Static website hosting:** Enabled
* **Index / Error documents:** `index.html` / `error.html`
* **Bucket policy:** Public `s3:GetObject` on all objects
* **Website endpoint:** `http://vineet-static-site.s3-website.<region>.amazonaws.com`

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`static_website_hosting_config.png`** — The **Properties → Static website hosting** panel of the bucket showing status **Enabled**, the index/error documents, and the **Bucket website endpoint** URL.
* **`static_website_hosting_active.png`** — A **browser window** loaded at the website endpoint URL showing the actual rendered `index.html`. The URL bar must be visible in the screenshot.

*(Optional video: navigate to the endpoint, then to a wrong path, so both index and error pages are seen.)*
