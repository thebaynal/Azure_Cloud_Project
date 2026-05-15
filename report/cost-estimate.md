# Cost Estimate Report

**Project:** Cloud Web Application Deployment on Azure  
**Team:** [Team Name]  
**Date:** [YYYY-MM-DD]  
**Report Period:** Monthly estimate (based on 24/7 usage)

---

## Executive Summary

This report provides a detailed cost analysis for the deployed Azure cloud architecture. Using the Azure Pricing Calculator, we estimated a monthly cost of **$[TOTAL_COST]** for our baseline deployment and **$[OPTIMIZED_COST]** for the optimized configuration with implemented cloud improvements.

---

## Architecture Overview

### Baseline Deployment Architecture

**Selected Scenario:** [School Portal / E-Commerce / Enrollment System / Blog Platform / Custom]

**Core Components:**
- **Compute:** 2x App Service instances (B1 tier)
- **Database:** Azure SQL Database (Basic tier, 5 DTU)
- **Storage:** Azure Storage Account with Zone-Redundant Storage (ZRS)
- **Networking:** Virtual Network, Network Security Groups
- **Monitoring:** Application Insights (limited)

**Geographic Distribution:** 
- Primary Region: [Southeast Asia / East US / etc.]
- Redundancy Model: [Zone-Redundant / Single Zone]

---

## Itemized Cost Breakdown

### Monthly Cost Estimate

| Service | Configuration | Unit Cost | Monthly Usage | Monthly Cost |
|---------|---------------|-----------|-----------|---------:|
| **Compute** | | | | |
| App Service Plan (B1) | 2 instances | $0.0743/hour | 720 hours × 2 | $106.75 |
| **Database** | | | | |
| Azure SQL Database | Basic tier, 5 DTU | $5.73/month | Single DB | $5.73 |
| Storage (Database) | 2 GB included | Included | — | $0.00 |
| **Storage** | | | | |
| Blob Storage (Hot) | Zone-Redundant (ZRS) | $0.047/GB | 100 GB | $4.70 |
| Storage Transactions | Read/Write operations | $0.0004 per 10k | 1M operations | $4.00 |
| **Networking** | | | | |
| Data Transfer (Outbound) | Internet egress | $0.087/GB | 50 GB | $4.35 |
| Virtual Network | Basic features | Free | — | $0.00 |
| **Monitoring & Management** | | | | |
| Application Insights | Pay-as-you-go | $2.99/GB ingested | 5 GB/month | $14.95 |
| Azure Monitor | Metrics & Alerts | Free | — | $0.00 |
| **Security** | | | | |
| Azure Key Vault | 1 million operations | $0.03 per 10k ops | 50k operations | $0.15 |
| Network Security Groups | Basic rules | Free | — | $0.00 |
| **Total Estimated Monthly Cost** | | | | **$140.63** |

---

## Cost Optimization Analysis

### Current Optimization Strategies Implemented

#### 1. **Right-Sizing with Reserved Instances (Potential Savings: 25-35%)**
- **Strategy:** Use 1-year or 3-year reserved instances instead of pay-as-you-go
- **Current Cost:** $106.75/month for App Service Plan
- **With Reservation:** $69–$80/month (1-year commitment)
- **Monthly Savings:** $27–$37
- **Status:** [Implemented / Not Yet Implemented]

#### 2. **Auto-Scaling Based on Demand (Potential Savings: 15-20%)**
- **Strategy:** Implement autoscale rules to reduce instances during off-peak hours
- **Implementation:** 2 instances peak hours (9 AM–6 PM) → 1 instance off-peak
- **Estimated Savings:** ~$15–$20/month
- **Status:** [Implemented / Recommended for next phase]

#### 3. **Serverless Architecture for Background Tasks (Potential Savings: 10-15%)**
- **Strategy:** Migrate background jobs to Azure Functions (consumption-based pricing)
- **Potential Resources:** File processing, email notifications, data exports
- **Estimated Savings:** $10–$15/month
- **Status:** [Evaluated / Not implemented]

#### 4. **Database Tier Optimization (Potential Savings: $0–$5/month)**
- **Current:** Azure SQL Database (Basic tier, 5 DTU)
- **Alternative:** Azure Database for PostgreSQL Flexible Server (equivalent specs, similar cost)
- **Decision Rationale:** [Current choice meets requirements and cost-effective]

#### 5. **Storage Optimization (Potential Savings: $1–$3/month)**
- **Strategy:** Move infrequently accessed data to Cool or Archive tier
- **Current Tier:** Hot (frequent access)
- **Archive Tier:** $0.01/GB/month (82% reduction)
- **Trade-off:** Retrieval costs and latency penalties
- **Status:** [Not applicable / Suitable for archival only]

---

## Cost Projection (Annual)

| Period | Configuration | Estimated Cost |
|--------|---------------|---------:|
| Current Month | Baseline (2 instances, pay-as-you-go) | $140.63 |
| 12-Month Projection | Baseline (continuous) | $1,687.56 |
| **With Optimization** | 1-year reserved instances + autoscaling | $800–$1,000 |
| **Annual Savings** | Reserved instances + autoscaling | $687–$887 (~50% reduction) |

---

## Azure Pricing Calculator Screenshot

[Insert screenshot of Azure Pricing Calculator below]

**How to verify:**
1. Visit: https://azure.microsoft.com/en-us/pricing/calculator/
2. Search for each service (App Service, SQL Database, Storage, etc.)
3. Configure with same parameters as listed in "Itemized Cost Breakdown"
4. Screenshot the final estimate
5. Embed or link screenshot here

---

## Cost Monitoring & Alerts

### Implemented Monitoring
- **Cost Management + Billing Dashboard:** Set up to track spending against budget
- **Budget Alert:** Alert if monthly spending exceeds $200
- **Resource-Level Tagging:** All resources tagged by project, environment, and cost center

### Recommended Monitoring
```bash
# Command to check current month cost
az costmanagement forecast --timeframe MonthToDate --granularity Daily
```

---

## Recommendations

### Short-Term (Next Month)
1. ✅ [Recommendation 1: e.g., Implement auto-scaling rules]
2. ✅ [Recommendation 2: e.g., Purchase 1-year reserved instances]
3. ✅ [Recommendation 3: e.g., Migrate static content to CDN]

### Medium-Term (Next Quarter)
1. 📋 [Recommendation: Evaluate serverless functions]
2. 📋 [Recommendation: Optimize database transactions]
3. 📋 [Recommendation: Implement caching strategy]

### Long-Term (Next Year)
1. 🔮 [Recommendation: Move to managed services where applicable]
2. 🔮 [Recommendation: Evaluate multi-region deployment for high availability]

---

## Compliance & Credits

- **Azure for Students Credit Used:** $[AMOUNT] / $100/year
- **Remaining Credit:** $[AMOUNT]
- **Project Completion Deadline:** [Date]
- **Resources to be Deleted:** [Scheduled date to avoid charges]

---

## Cost Optimization Decision Matrix

| Optimization Strategy | Implementation Effort | Potential Savings | Risk Level | Recommended |
|---|---|---|-|-|
| Reserved Instances | Low | 25-35% | Very Low | ✅ Yes |
| Auto-Scaling | Medium | 15-20% | Low | ✅ Yes |
| Serverless Functions | High | 10-15% | Medium | 📋 Phase 2 |
| CDN for Static Content | Medium | 5-10% | Low | 📋 If applicable |
| Database Tier Reduction | Low | 0-5% | Medium | ❌ Not needed |
| Archive Storage | High | 20-30% (archive only) | High | ❌ Not applicable |

---

## Conclusion

Our deployed architecture balances **performance, reliability, and cost-effectiveness**. By implementing the recommended optimizations, we can achieve **~50% cost reduction** while maintaining the same performance characteristics. The current baseline cost of **$140.63/month** is well within the Azure for Students monthly credit allocation.

**Key Takeaway:** The primary cost driver is the **App Service Plan ($106.75/month)**. Reserved instances and intelligent auto-scaling provide the best ROI for cost optimization.

---

## Appendix: Azure Pricing Calculator Estimate

```
[Insert complete pricing calculator configuration and results here]

Example:
- Service: App Service Plan
- Region: Southeast Asia
- Tier: Basic (B1)
- Instance Count: 2
- Compute Hours: 1,440/month
- Monthly Cost: $106.75

- Service: Azure SQL Database
- Region: Southeast Asia
- Service Tier: Basic (5 DTU)
- Monthly Cost: $5.73

[Continue for all services...]
```

---

**Report Prepared By:** [Team Member Name]  
**Date:** [YYYY-MM-DD]  
**Last Updated:** [YYYY-MM-DD]  
**Status:** Final / Draft
