# AWS Service Setup Guide: Lambda

## 1. Overview
AWS Lambda runs code without provisioning any server — you upload a function, AWS runs it on demand and only bills for execution time. During the internship a simple Python Lambda function was created, tested from the console with a sample event, then exposed via a **Function URL** so it could be invoked over HTTPS from a browser.

## 2. Step-by-Step Setup Guide
1. Open the **Lambda** console → **Create function**.
2. Author from scratch. Function name: `vineet-hello`. Runtime: **Python 3.12**. Architecture: x86_64.
3. Permissions: **Create a new role with basic Lambda permissions** (writes logs to CloudWatch).
4. Create function.
5. In the **Code** tab, replace the default handler with:
   ```python
   import json
   def lambda_handler(event, context):
       qs = event.get("queryStringParameters") or {}
       name = qs.get("name", "World")
       return {
           "statusCode": 200,
           "headers": {"Content-Type": "application/json"},
           "body": json.dumps({"message": f"Hello, {name}, from Vineet's Lambda!"})
       }
   ```
6. Click **Deploy**.
7. **Test** tab → create a test event `hello-event` with body `{}` → **Test**. Confirm the response shows `"Hello, World"`.
8. **Configuration → Function URL → Create function URL**. Auth type: **NONE** (for the lab only). Create.
9. Open the generated Function URL in a browser — the JSON greeting should render.
10. Try `?name=Vineet` at the end of the URL to see the argument reflected in the response.
11. Open **CloudWatch → Log groups → /aws/lambda/vineet-hello** to view the execution logs.

## 3. Key Configurations Used
* **Function name:** `vineet-hello`
* **Runtime:** Python 3.12, x86_64, 128 MB, 3-second timeout
* **IAM role:** auto-created execution role with CloudWatch Logs permissions
* **Trigger:** Function URL, Auth = NONE (lab only)
* **Test event:** `hello-event` with body `{}`

## 4. Implementation Proof & Verification
> [!NOTE]
> Below are the visual confirmations of the successful setup and configuration.

### Screenshots to capture
* **`lambda_config.png`** — The Lambda function's **Code + Configuration** page showing the function name, runtime, and the Function URL displayed under **Configuration → Function URL**.
* **`lambda_active.png`** — A **browser window** at the Function URL (with `?name=Vineet`) showing the JSON `{"message": "Hello, Vineet, from Vineet's Lambda!"}` response. The URL bar must be visible.

*(Optional video: invoke the console **Test** button, then open the Function URL in a browser, then open the CloudWatch log group to show the corresponding log lines.)*
