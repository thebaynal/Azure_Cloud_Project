# Azure Deployment Documentation

## Overview
This document provides step-by-step instructions for deploying the MaScan QR Attendance Checker application on Microsoft Azure.

## Prerequisites
- Azure for Students account (or paid subscription)
- Flask application code
- Azure Portal access
- Local Python 3.10+ environment

## Deployment Architecture

The application is deployed across the following Azure services:

| Resource | Type | Location | Purpose |
|----------|------|----------|---------|
| mascan-rg | Resource Group | Southeast Asia | Container for all resources |
| mascan-qr | App Service | Southeast Asia | Host Flask web application |
| ASP-mascarqr | App Service Plan | Southeast Asia | Compute resources for App Service |
| mascan-sql | Azure SQL Database | Southeast Asia | Production database |
| mascanstorage | Storage Account | Southeast Asia | Blob storage for uploads |

## Step-by-Step Deployment Instructions

### Step 1: Create Resource Group
![Step 1](./screenshots/01-resource-group.png)
- Navigate to Azure Portal → Resource Groups
- Click "Create"
- Name: `mascan-rg`
- Region: Southeast Asia
- Click "Review + Create" → "Create"

**Time**: ~2 minutes

### Step 2: Create Azure SQL Database
![Step 2](./screenshots/02-sql-database.png)
- Go to Resource Group (mascan-rg)
- Click "Create" → Search for "SQL Database"
- Database name: `mascan-db`
- Server: Create new
  - Server name: `mascan-server` (must be globally unique)
  - Admin login: `dbadmin`
  - Password: *[Use strong password]*
  - Location: Southeast Asia
- Pricing: Basic tier ($5/month) or Standard
- Click "Review + Create" → "Create"

**Configure Firewall:**
- In SQL Server settings → Firewall and virtual networks
- Add your IP address
- Allow Azure services: ON

**Time**: ~5 minutes

### Step 3: Create Storage Account
![Step 3](./screenshots/03-storage-account.png)
- Go to Resource Group (mascan-rg)
- Click "Create" → Search for "Storage Account"
- Storage account name: `mascanstorage` (must be globally unique, lowercase)
- Region: Southeast Asia
- Redundancy: Locally-redundant storage (LRS)
- Performance: Standard
- Click "Review + Create" → "Create"

**Create Blob Container:**
- In Storage Account → Containers
- Click "Container"
- Name: `uploads`
- Public access level: Private
- Click "Create"

**Time**: ~3 minutes

### Step 4: Create App Service Plan
![Step 4](./screenshots/04-app-service-plan.png)
- Go to Resource Group (mascan-rg)
- Click "Create" → Search for "App Service Plan"
- Name: `ASP-mascarqr`
- Region: Southeast Asia
- OS: Windows
- Sku and size: **B2** (Standard) - enables autoscaling
- Click "Review + Create" → "Create"

**Why B2?** Free/B1 tiers don't support autoscaling or multiple instances

**Time**: ~2 minutes

### Step 5: Create App Service Instance 1
![Step 5](./screenshots/05-app-service-instance-1.png)
- Go to Resource Group (mascan-rg)
- Click "Create" → Search for "App Service"
- Name: `mascan-qr`
- Runtime: Python 3.10
- App Service Plan: ASP-mascarqr
- Click "Review + Create" → "Create"

**Deployment:**
- Go to App Service (mascan-qr) → Deployment Center
- Source: GitHub (connect your repo)
- Organization: Select your GitHub account
- Repository: QR-Attendance-Checker---Flask-Version
- Branch: main
- Click "Save"
- Azure will automatically deploy when you push to main branch

**Configure Application Settings:**
- Go to Configuration → Application settings
- Add these environment variables:
  ```
  FLASK_ENV=production
  DATABASE_URL=Server=mascan-server.database.windows.net;Database=mascan-db;User Id=dbadmin;Password=YOUR_PASSWORD;
  STORAGE_ACCOUNT_NAME=mascanstorage
  STORAGE_ACCOUNT_KEY=YOUR_STORAGE_KEY
  WEBSITES_PORT=8000
  ```
- Click "Save"

**Time**: ~5 minutes

### Step 6: Create App Service Instance 2 (for fault tolerance)
![Step 6](./screenshots/06-app-service-instance-2.png)
- Go to Resource Group (mascan-rg)
- Click "Create" → Search for "App Service"
- Name: `mascan-qr-2`
- Runtime: Python 3.10
- App Service Plan: ASP-mascarqr (same plan = same instances)
- Click "Review + Create" → "Create"
- Repeat deployment and configuration from Step 5

**Result**: Same App Service Plan now runs 2 instances for load balancing

**Time**: ~5 minutes

### Step 7: Configure Application Gateway (Load Balancer)
![Step 7](./screenshots/07-app-gateway.png)
- Go to Resource Group (mascan-rg)
- Click "Create" → Search for "Application Gateway"
- Name: `mascan-gateway`
- SKU: Standard_v2 (supports autoscaling)
- Tier: Standard
- Instance count: 2
- Autoscale: Enable (Min 2, Max 5)
- Virtual Network: Create new or use existing
- Click "Next" → Configure backends:
  - Backend pool name: `mascan-pool`
  - Add both App Services (mascan-qr, mascan-qr-2)
- Click "Next" → Configure routing rules:
  - Listener name: `default`
  - Frontend IP: Public
  - Port: 80 (HTTP)
  - Backend target: mascan-pool
  - HTTP settings: Create new
- Click "Review + Create" → "Create"

**Get Gateway Public IP:**
- Go to Application Gateway → Frontend public IP
- Copy the IP address - this is your app URL: `http://YOUR_IP`

**Time**: ~10 minutes

### Step 8: Configure Autoscale Rules
![Step 8](./screenshots/08-autoscale.png)
- Go to App Service Plan (ASP-mascarqr) → Scale out
- Click "Enable autoscale"
- Default rule:
  - When metric: CPU Percentage
  - Operator: Greater than
  - Threshold: 70
  - Action: Increase instance count by 1
- Add scale-in rule:
  - When metric: CPU Percentage
  - Operator: Less than
  - Threshold: 30
  - Action: Decrease instance count by 1
- Maximum instances: 5
- Click "Save"

**Result**: Your app automatically scales from 2-5 instances based on CPU load

**Time**: ~3 minutes

### Step 9: Configure Application Insights (Monitoring)
![Step 9](./screenshots/09-application-insights.png)
- Go to App Service (mascan-qr) → Application Insights
- Click "Turn on Application Insights"
- Create new: `mascan-insights`
- Runtime: Python
- Location: Southeast Asia
- Click "Apply"

**Verify Telemetry:**
- Navigate to Application Insights → Overview
- You should see request counts, response times, and failure rates
- Check "Application map" to see dependencies

**Time**: ~2 minutes

## Verification Checklist

✅ Resource Group created in Southeast Asia  
✅ Azure SQL Database accessible (firewall configured)  
✅ Storage Account with blob container  
✅ App Service Plan set to B2 (or higher)  
✅ Both App Service instances running  
✅ Application Gateway routing traffic  
✅ Autoscale rules configured (2-5 instances)  
✅ Application Insights monitoring enabled  
✅ Environment variables configured  
✅ App deployed and running at Application Gateway IP  

## Testing Your Deployment

1. **Access your app:**
   ```
   http://YOUR_APP_GATEWAY_IP
   ```

2. **Test functionality:**
   - Log in with admin credentials
   - Create an event
   - Upload sample CSV file
   - Verify QR scanning works
   - Export PDF attendance report

3. **Check load balancing:**
   - Go to App Service Plan → Instances
   - Verify both instances are running and healthy

4. **Monitor performance:**
   - Go to Application Insights → Performance
   - Check response times and request volumes

## Troubleshooting

### App not loading
- Check App Service deployment logs: App Service → Deployment Center → Logs
- Verify environment variables are set correctly
- Check database connection string format

### Database connection fails
- Verify SQL Server firewall allows your IP
- Check connection string in Application Settings
- Test connection locally first

### High latency/timeouts
- Check Application Gateway backend health probes
- Review Application Insights for slow dependencies
- Monitor CPU percentage and scale-out events

## Cost Optimization

- **Reserved Instances**: Save 30-40% on App Service Plan with 1-year commitment
- **Autoscale**: Only pay for instances you use
- **Storage Tiering**: Move old files to Archive tier
- **Turn off during dev**: Delete resources when not in use

## Clean Up (After Grading)

To avoid charges, delete all resources:

```powershell
# Delete entire resource group (all resources inside)
az group delete --name mascan-rg --yes
```

---

**Last Updated**: 2026-05-12  
**Team Members**: [Add team member names]  
**Demo URL**: [Add your Application Gateway IP URL]
