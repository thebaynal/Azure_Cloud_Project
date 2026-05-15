# Deployment Documentation

This directory contains all deployment instructions and resources for the Azure Cloud Project.

---

## 📋 Deployment Overview

**Architecture Type:** [Baseline / Optimized]  
**Deployment Method:** [Azure CLI / Azure Bicep / Azure Portal GUI]  
**Target Region:** [e.g., Southeast Asia]  
**Resource Group:** [e.g., rg-cloud-project-prod]

---

## 🔧 Prerequisites

Before beginning deployment, ensure you have:

- ✅ Azure for Students account with active credits
- ✅ Azure CLI installed (`az --version` to verify)
- ✅ Azure PowerShell (optional, for advanced operations)
- ✅ Git configured with GitHub credentials
- ✅ Appropriate permissions in your Azure subscription

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

## 🚀 Deployment Options

### Option A: Using Azure CLI Script

1. **Authenticate with Azure:**
   ```bash
   az login
   ```
   Follow the browser prompt and select your Azure for Students account.

2. **Verify your subscription:**
   ```bash
   az account show
   ```

3. **Review deployment script:**
   - Open `deploy.azcli` in your text editor
   - Update variable values (resource names, regions, etc.)
   - Ensure no hardcoded passwords are present

4. **Execute the deployment script:**
   ```bash
   # On Linux/macOS
   chmod +x deploy.azcli
   ./deploy.azcli

   # On Windows PowerShell
   .\deploy.azcli
   ```

5. **Monitor deployment progress:**
   - Check the Azure Portal for resource creation status
   - Wait for all resources to transition to "Succeeded" status

6. **Post-deployment validation:**
   ```bash
   az resource list --resource-group [YOUR_RESOURCE_GROUP] --output table
   ```

### Option B: Using Azure Portal GUI

Follow the step-by-step screenshots in the `screenshots/` folder:

1. [Step 01] Create Resource Group
2. [Step 02] Deploy App Service Plan
3. [Step 03] Deploy App Service (x2 instances)
4. [Step 04] Configure Database
5. [Step 05] Set up Storage Account
6. [Step 06] Configure Security and NSGs
7. [Step 07] Deploy Load Balancer (if applicable)
8. [Step 08] Configure Monitoring

For each screenshot, follow the highlighted steps and settings shown.

---

## 📊 Resources Deployed

| Resource Type | Resource Name | Configuration |
|---------------|---------------|---|
| Resource Group | [rg-name] | [Region] |
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

## 👥 Deployment Log

| Date | Member | Action | Status |
|------|--------|--------|--------|
| [YYYY-MM-DD] | [Name] | Created Resource Group | ✅ |
| [YYYY-MM-DD] | [Name] | Deployed App Service Plan | ✅ |
| [YYYY-MM-DD] | [Name] | Deployed Database | ✅ |

---

**Last Updated:** [YYYY-MM-DD]  
**Deployment Status:** [In Progress / Completed / Tested]
