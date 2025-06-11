# 🚀 CI/CD Using GitHub Webhook and PM2

This guide explains how to set up a **CI/CD pipeline** using GitHub Webhooks and manage the deployment with **PM2**.

---

## 🛠️ Step 1: Install `webhook`

```sh
sudo apt-get update && sudo apt-get install webhook
```

---

## 📝 Step 2: Create the Webhook Configuration File

Create the hooks configuration file:

```sh
nano ~/hooks.json
```

### Content of `hooks.json` (Webhook Endpoint Configuration)

```json
[
  {
    "id": "webhook-url-endpoint",
    "execute-command": "/home/ubuntu/deploy-scripts/deploy-service.sh",
    "command-working-directory": "/home/ubuntu",
    "trigger-rule": {
      "match": {
        "type": "payload-hmac-sha256",
        "secret": "long-secret-token-should-be-berry-harad-to-gussee",
        "parameter": {
          "source": "header",
          "name": "X-Hub-Signature-256"
        }
      }
    }
  }
]
```

---

## 📜 Step 3: Create the Deployment Script

Create the script that will be triggered by the webhook:

```sh
nano /home/ubuntu/deploy-scripts/deploy-service.sh
```

### Example content of `deploy-service.sh`

```bash
#!/bin/bash

# Pull the latest changes
cd /home/ubuntu/my-service
git pull origin main

# Install dependencies (optional)
npm install

# Restart the service using PM2
pm2 restart my-service
```

> 💡 Make sure the script is executable:
>
> ```sh
> chmod +x /home/ubuntu/deploy-scripts/deploy-service.sh
> ```

---

## ⚙️ Step 4: Run Webhook Server Using PM2

To run the webhook server in the background using `pm2`:

```sh
pm2 start webhook --name webhook-server -- \
  -hooks ~/hooks.json -verbose
```

> 💡 Install PM2 if you haven’t already:
>
> ```sh
> npm install -g pm2
> ```

---

## 🔗 GitHub Webhook Endpoint

Use the following URL as the webhook endpoint in your GitHub repository settings:

```
http://<YOUR_EC2_PUBLIC_IP>:9000/hooks/webhook-url-endpoint
```

✅ Don’t forget to add the **secret** used in `hooks.json` to the GitHub webhook settings to validate payloads securely.

---
