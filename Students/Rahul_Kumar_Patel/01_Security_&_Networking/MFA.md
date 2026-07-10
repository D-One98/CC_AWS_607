# AWS Service Setup Guide: MFA (Multi-Factor Authentication)

## 1. Overview
MFA adds a second layer of authentication on top of the password — a rotating 6-digit code from a virtual authenticator app (Google Authenticator, Authy, Microsoft Authenticator). During the internship MFA was enabled on both the **root user** and the **IAM lab user** so that even if a password leaked, the account could not be logged into without the phone.

## 2. Step-by-Step Setup Guide
1. Sign in to the AWS Console (as root, or as the IAM user for their own MFA).
2. Open **IAM → Users**, choose the user, and open the **Security credentials** tab.
3. Under **Multi-factor authentication (MFA)** click **Assign MFA device**.
4. Give the device a name (e.g., `Vineet-Phone`) and choose **Authenticator app**, then **Next**.
5. Open the authenticator app on the phone and scan the QR code displayed on the AWS page.
6. Enter **two consecutive** 6-digit codes from the app into the AWS page and click **Add MFA**.
7. Sign out and sign back in — AWS will now prompt for the MFA code after the password.
8. Repeat the entire flow for the **root user** from **My Security Credentials** to secure the root account.

## 3. Key Configurations Used
* **MFA type:** Virtual MFA device (authenticator app)
* **Device name:** `Vineet-Phone`
* **Users protected:** Root user + IAM user `vineet-admin`
* **Backup:** QR-code screenshot / recovery codes stored offline

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`mfa_config.png`** — The **Security credentials** tab of the IAM user showing the MFA device row with status **Assigned** and the device name visible (the secret/QR code itself should NOT be in the screenshot).
* **`mfa_active.png`** — The AWS **sign-in page prompting for the MFA code** (the "Additional verification required" screen) taken during a fresh login, proving MFA is enforced.

*(Optional video: a 15-second screen recording of a fresh login showing password → MFA prompt → dashboard.)*
