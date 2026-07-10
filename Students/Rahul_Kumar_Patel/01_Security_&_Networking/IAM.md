# AWS Service Setup Guide: IAM (Identity and Access Management)

## 1. Overview
AWS IAM is the service that controls **who (users/roles) can do what (permissions) on which resources** in an AWS account. During the internship it was used to create individual login users instead of sharing the root account, to group users by role, and to attach fine-grained policies so each service (EC2, S3, Lambda, etc.) only had the permissions it actually needed. This is the foundation of every other lab — nothing else is launched from the root user.

## 2. Step-by-Step Setup Guide
1. Sign in to the AWS Console as the **root user** and open the **IAM** service.
2. Go to **Users → Create user**, enter a username (e.g., `vineet-admin`), and enable **Provide user access to the AWS Management Console**.
3. Choose **I want to create an IAM user**, set an auto-generated or custom password, and leave "Users must create a new password at next sign-in" checked.
4. On the permissions page choose **Attach policies directly** and attach `AdministratorAccess` (for the lab user) or a job-specific policy (e.g., `AmazonEC2FullAccess`).
5. Optionally create a **Group** (e.g., `Developers`) and add the user to it so policies can be managed at group level.
6. Review and **Create user**. Download the `.csv` with the sign-in URL, username, and password.
7. Sign out of root, open the account-specific sign-in URL, and log in as the new IAM user for all further labs.
8. Under **Users → Security credentials**, create an **Access key** only if CLI/SDK access is required, and store it securely.

## 3. Key Configurations Used
* **User name:** `vineet-admin` (individual, non-shared)
* **Access type:** AWS Management Console access + programmatic access (only where CLI was needed)
* **Permissions:** `AdministratorAccess` managed policy for lab work; least-privilege custom policies for service-role users
* **Password policy:** Minimum 8 characters, require reset at first sign-in
* **Group membership:** `Developers` group (policies attached at group level where possible)

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`iam_config.png`** — The **IAM → Users** list page showing the newly created user (`vineet-admin`) with the attached policy/group visible in the row.
* **`iam_active.png`** — A screenshot taken **after signing in as the IAM user** (top-right of console shows the IAM username @ account ID) proving the credentials work.

*(Optional video: a 20–30 second screen recording of the create-user wizard from "Create user" through the final success screen.)*
