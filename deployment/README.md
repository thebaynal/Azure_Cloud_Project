# MaScan Deployment Guide — Azure

This guide provides step-by-step instructions for deploying the MaScan application to Microsoft Azure using Azure App Service and Azure Container Registry.

---

## 📋 Prerequisites

Before beginning deployment, ensure you have:

- ✅ **Azure for Students account** (free at https://azure.microsoft.com/en-us/free/students/)
- ✅ **Azure CLI installed** (verify with `az --version`)
- ✅ **Docker Desktop installed**
- ✅ **Git installed**

### Install Azure CLI

```bash
# Windows
msiexec /i https://aka.ms/InstallAzureCliBundledWindows

# macOS
brew install azure-cli

# Linux
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

---

## 🚀 Step-by-Step Deployment

### Step 1 — Log in to Azure

Open a terminal and authenticate with your Azure account:

```bash
az login
```

A browser window will open. Sign in with your school email (Azure for Students account).

---

### Step 2 — Create a Resource Group

Create a resource group to organize all your resources:

```bash
az group create --name mascan-rg --location eastus
```

> You can change `eastus` to a region closer to you:
> - `southeastasia`, `eastasia`, `westeurope`, `centralus`, etc.

---

### Step 3 — Create an Azure Container Registry (ACR)

The Container Registry stores your Docker images.

```bash
az acr create --resource-group mascan-rg --name mascanregistry1111 --sku Basic --admin-enabled true
```

> If `mascanregistry1111` is already taken, use a unique name like `mascan2025registry`.

Retrieve your ACR credentials (you'll need these in Step 8):

```bash
az acr credential show --name mascanregistry1111
```

**Note:** Save the username and password for later use.

---

### Step 4 — Build and Push Docker Image

From the root of the repository, build and push the Docker image to ACR:

```bash
# Log in to your container registry
az acr login --name mascanregistry1111

# Build the image
docker build -t mascanregistry1111.azurecr.io/mascan:latest .

# Push the image to ACR
docker push mascanregistry1111.azurecr.io/mascan:latest
```

---

### Step 5 — Create Azure App Service Plan

Create an App Service Plan (B1 is free tier eligible for Azure for Students):

```bash
az appservice plan create \
  --name mascan-plan \
  --resource-group mascan-rg \
  --is-linux \
  --sku B1
```

---

### Step 6 — Create the Web App

Create the Web App and configure it to use the Docker image from ACR:

```bash
az webapp create \
  --resource-group mascan-rg \
  --plan mascan-plan \
  --name mascan-app \
  --deployment-container-image-name mascanregistry1111.azurecr.io/mascan:latest
```

> If `mascan-app` is already taken, use something unique like `mascan-attendance-2025`.

---

### Step 7 — Configure Environment Variables

Set application environment variables:

```bash
az webapp config appsettings set \
  --resource-group mascan-rg \
  --name mascan-app \
  --settings \
    SECRET_KEY="replace-with-a-long-random-secret" \
    DB_PATH="/home/data/mascan_attendance.db" \
    SESSION_FILE_DIR="/home/flask_session" \
    WEBSITES_PORT=8000
```

**Generate a strong SECRET_KEY:**

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Replace the `SECRET_KEY` value above with the output.

---

### Step 8 — Enable Persistent Storage

Configure App Service to persist data across restarts:

```bash
az webapp config appsettings set \
  --resource-group mascan-rg \
  --name mascan-app \
  --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
```

---

### Step 9 — Configure Container Registry Authentication

Give App Service access to pull images from ACR:

```bash
az webapp config container set \
  --name mascan-app \
  --resource-group mascan-rg \
  --docker-custom-image-name mascanregistry1111.azurecr.io/mascan:latest \
  --docker-registry-server-url https://mascanregistry1111.azurecr.io \
  --docker-registry-server-user mascanregistry1111 \
  --docker-registry-server-password "<password-from-step-3>"
```

Replace `<password-from-step-3>` with the password you saved in Step 3.

---

### Step 10 — Launch the Application

Open your app in the browser:

```bash
az webapp browse --resource-group mascan-rg --name mascan-app
```

Your app will be live at:

```
https://mascan-app.azurewebsites.net
```

---

## 🔄 Updating the Application

After making code changes, rebuild and redeploy:

```bash
docker build -t mascanregistry1111.azurecr.io/mascan:latest .
docker push mascanregistry1111.azurecr.io/mascan:latest
az webapp restart --resource-group mascan-rg --name mascan-app
```

---

## 🔍 Troubleshooting

### View application logs

```bash
az webapp log tail --resource-group mascan-rg --name mascan-app
```

### Restart the application

```bash
az webapp restart --resource-group mascan-rg --name mascan-app
```

### Check deployment status

```bash
az resource list --resource-group mascan-rg --output table
```

---

## 💰 Cost Estimate (Azure for Students)

| Resource | Monthly Cost |
|----------|---|
| App Service B1 | ~$13 (or free with student credits) |
| Container Registry Basic | ~$5 |
| Storage | Negligible |

**Azure for Students provides $100 free credits** — sufficient to run this deployment for several months.

---

## 📚 Additional Resources

- [Azure App Service Documentation](https://learn.microsoft.com/azure/app-service/)
- [Azure Container Registry Docs](https://learn.microsoft.com/azure/container-registry/)
- [Azure CLI Reference](https://learn.microsoft.com/cli/azure/)
- Main guide: [README-AZURE.md](../README-AZURE%20(1).md)
- Technical report: [report.md](../report/report.md)
| App Service Plan | [asp-name] | [Pricing Tier] |
| App Service | [app-name-1] | [Instance 1] |
| App Service | [app-name-2] | [Instance 2] |
| Database | [db-name] | [Type: SQL/Cosmos] |
| Storage Account | [sa-name] | [Redundancy: ZRS/GRS] |
| Security | [Details] | [NSG/WAF/etc] |

---

## 🔐 Security Checklist

- ✅ No passwords hardcoded in scripts
- ✅ Managed Identities configured for service-to-service authentication
- ✅ NSG rules restrict traffic to necessary ports only
- ✅ Storage account has SAS tokens or Managed Identity access
- ✅ Database credentials stored in Key Vault (if applicable)
- ✅ Application Insights configured for monitoring
- ✅ HTTPS/TLS enforced on all endpoints

---

## ✅ Post-Deployment Validation

1. **Test application accessibility:**
   ```bash
   curl https://[app-service-url]
   ```

2. **Verify load balancing:**
   - Access application multiple times
   - Check Application Insights logs for distribution across instances

3. **Test failover (if applicable):**
   - Stop one App Service instance
   - Verify traffic routes to healthy instance

4. **Monitor resource health:**
   - Open Azure Portal
   - Check "Health" status for all resources
   - Review Application Insights metrics

5. **Validate security:**
   - Test restricted access rules
   - Verify NSG blocks unauthorized ports
   - Confirm SSL/TLS certificate validity

---

## 🧹 Cleanup Instructions

**Important:** Delete all resources after project completion to avoid charges.

```bash
# List all resources in resource group
az resource list --resource-group [YOUR_RESOURCE_GROUP] --output table

# Delete entire resource group (will delete all resources within it)
az group delete --name [YOUR_RESOURCE_GROUP] --yes --no-wait
```

**Verification:**
```bash
az group list --output table
```
Ensure your resource group no longer appears in the list.

---

## 🐛 Troubleshooting

### Issue: "Insufficient quota" error
**Solution:** Check your Azure for Students credit balance and quota limits
```bash
az vm list-usage --location [region] --output table
```

### Issue: App Service deployment fails
**Solution:** Verify runtime stack compatibility and check deployment logs
```bash
az webapp log tail --name [app-name] --resource-group [rg-name]
```

### Issue: Database connection timeout
**Solution:** Verify NSG rules and firewall settings allow connection from App Service
```bash
az sql server firewall-rule list --resource-group [rg-name] --server [server-name]
```

### Issue: "Resource already exists" error
**Solution:** Use unique naming or clean up existing resources
```bash
az resource list --resource-group [YOUR_RESOURCE_GROUP] --query "[?name=='[resource-name]']"
```

---

## 📞 Support & Documentation

- **Azure CLI Reference:** https://learn.microsoft.com/en-us/cli/azure/
- **Azure Bicep Documentation:** https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/
- **Azure Architecture Center:** https://learn.microsoft.com/en-us/azure/architecture/
- **Troubleshooting Guide:** https://learn.microsoft.com/en-us/troubleshoot/azure/

---

**Last Updated:** [2026-5-16]  
**Deployment Status:** [Completed]
