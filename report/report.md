# Technical Report: MaScan — Cloud Architecture & Deployment

**Project:** MaScan — Cloud Web Application Deployment on Azure
**Course:** CSEC 3 – Cloud Computing
**Team Members:** MaScan Team
**Date:** 2026-05-16

---

## 1. Introduction

This report documents the MaScan cloud deployment on Microsoft Azure, covering architecture, deployment steps, security, monitoring, cost estimates, and recommendations for future improvements. The objective is a low-cost, maintainable deployment suitable for student projects and small production workloads.

**Selected Scenario:** Student attendance / lab access portal (dockerized Flask app with SQLite for demo)

---

## 2. Architecture Design

### 2.1 Baseline Architecture

Components:
- **Container Registry:** Azure Container Registry (Basic SKU) for storing Docker images.
- **App Platform:** Azure App Service (Linux) running a Docker container from ACR.
- **Persistent Storage:** App Service `/home` mount for SQLite DB (suitable only for low-concurrency/demo use).
- **Monitoring:** Application Insights + Azure Monitor for logs/alerts.
- **CI/CD:** GitHub Actions or Azure Pipelines to build and push images to ACR.

Data flow summary:
1. Developers push code to repository.
2. CI builds Docker image and pushes to `mascanregistry1111` (ACR).
3. App Service pulls the Docker image from ACR and runs the container.
4. Application writes local DB to `/home/data/mascan_attendance.db` and serves web traffic.

### 2.2 Design Rationale

- SQLite on App Service keeps costs minimal and simplifies setup for demos. It is NOT suitable for heavy concurrent writes or multi-instance scaling.
- Using ACR + App Service simplifies deployment and provides a path to replace the DB with Azure SQL or PostgreSQL when scaling.

---

## 3. Deployment Strategy

### 3.1 Infrastructure-as-Commands

Primary tooling: Azure CLI and Docker (scripts provided in `deployment/deploy.azcli`). Key steps from `README-AZURE.md`:

1. `az login`
2. `az group create --name mascan-rg --location eastus`
3. `az acr create --resource-group mascan-rg --name mascanregistry1111 --sku Basic --admin-enabled true`
4. Build and push the Docker image (see below) to ACR.
5. Create an App Service Plan (B1) and a Web App configured to use the ACR image.
6. Configure application settings and enable App Service storage.

Deployment validation checklist:
- App URL returns HTTP 200 and expected UI
- Environment variables present in App Service configuration
- Persistent DB file present in `/home/data/`
- Logs available via `az webapp log tail`

### 3.2 Build & Deploy (CI/CD)

Suggested pipeline steps (GitHub Actions):
- Build Docker image
- Login to ACR (via `az acr login` or `docker login` with service principal)
- Push image to ACR
- Update App Service container settings or slot deployment

---

## 4. Security

### 4.1 Controls

- **Secrets:** Use Azure Key Vault to store `SECRET_KEY` and any DB/password-like secrets; use managed identity to access Key Vault from App Service.
- **Transport:** TLS is provided by App Service (HTTPS).
- **Network:** Use App Service access restrictions if limiting IP ranges is needed.

### 4.2 Recommendations

1. Rotate `SECRET_KEY` and do not store it in source code.
2. Use managed identities and Key Vault for any credentials.
3. Limit diagnostic log retention and sanitize PII before logging.

---

## 5. Monitoring & Observability

### 5.1 Recommended Metrics

- HTTP response time (p95)
- Request rate
- Error rate (4xx / 5xx)
- CPU / Memory for container
- Free disk space on `/home`

### 5.2 Alerts

- Alert on high error rate (>1% sustained), high CPU (>80% 5m), low disk (<200MB free), and budget thresholds.

Commands:
```powershell
az webapp log tail --resource-group mascan-rg --name mascan-app
az monitor metrics list --resource /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/mascan-rg/providers/Microsoft.Web/sites/mascan-app --metric CPUPercentage
```

---

## 6. Cost Summary

Refer to [cost-estimate.md](cost-estimate.md) for a line-item breakdown and recommendations. Key points:

- Baseline demo deployment: low-cost (single B1 instance + ACR)
- Primary variable costs are App Service compute and outbound bandwidth
- Use Azure for Students credits and application sampling to reduce costs

---

## 7. Lessons Learned & Recommendations

### 7.1 Technical Challenges

- **SQLite persistence limits:** The App Service `/home` mount persists across restarts but is not suited for high concurrency. Solution: plan migration to Azure Database when needed.
- **Image access permissions:** App Service requires correct ACR credentials or system-assigned managed identity with `acrpull` role.

### 7.2 Best Practices Adopted

1. Use environment variables for all runtime configuration.
2. Enable Application Insights with sampling in non-debug environments.
3. Tag resources by `project`, `environment`, and `owner` for cost tracking.

### 7.3 Operational Recommendations

1. Add a lightweight backup/export job to periodically export the SQLite DB to Blob Storage.
2. Implement autoscale (or move to scale-to-zero services) before increasing user concurrency.
3. Add scheduled cleanup of old logs and artifacts to control storage costs.

---

## 8. Appendix — Commands & Useful Links

- Deploy script: [deployment/deploy.azcli](../deployment/deploy.azcli)
- README / quick-start: [README-AZURE.md](../README-AZURE.md)
- Azure docs: https://learn.microsoft.com/azure/

---

**Report Prepared By:** MaScan Team  
**Date:** 2026-05-16  
**Status:** Final

