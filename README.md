# MaScan — QR Attendance Checker on Azure Cloud

[![Deployment Status](https://img.shields.io/badge/status-production-brightgreen)](https://mascan-qr-acexb0a8febaacab.southeastasia-01.azurewebsites.net/login)
[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Flask](https://img.shields.io/badge/Flask-2.x-lightgrey.svg)](https://flask.palletsprojects.com/)
[![Azure](https://img.shields.io/badge/Cloud-Azure-0078D4.svg)](https://azure.microsoft.com/)

A cloud-deployed QR code-based attendance tracking system built with Flask and hosted on Microsoft Azure.

---

## 📋 Table of Contents

- [Quick Start](#quick-start)
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Team & Credits](#team--credits)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Deployment Guide](#deployment-guide)
- [Cost Estimation](#cost-estimation)
- [Documentation](#documentation)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

---

## 🚀 Quick Start

### Live Demo
- **Application**: [MaScan Login Portal](https://mascan-qr-acexb0a8febaacab.southeastasia-01.azurewebsites.net/login)
- **Video Demo**: [YouTube Walkthrough](https://youtu.be/sHimFjR4bxU)

### Deploy in 10 Minutes
```bash
# Clone the repository
git clone https://github.com/thebaynal/Azure_Cloud_Project.git
cd Azure_Cloud_Project

# Follow the deployment guide
cat deployment/README.md
```

---

## 📖 Project Overview

**MaScan** is a student attendance management system that leverages QR codes for efficient check-in and tracking. Deployed on Microsoft Azure, it demonstrates cloud-native architecture, scalability, and security best practices for educational institutions.

### Use Cases
- **Educational Institutions**: Track student attendance in lectures, labs, and seminars
- **Event Management**: Manage attendee check-ins at conferences and workshops
- **Access Control**: Monitor and log facility access using QR codes

---

## 🏗️ Architecture

### Deployment Architecture

The application is deployed across the following Azure services:

| Resource | Type | Location | Purpose |
|----------|------|----------|---------|
| `mascan-rg` | Resource Group | Southeast Asia | Centralized resource container with proper RBAC |
| `mascan-qr` | App Service | Southeast Asia | Primary Flask web application instance |
| `mascan-qr-2` | App Service | Southeast Asia | Secondary instance for high availability |
| `ASP-mascarqr` | App Service Plan | Southeast Asia | B2 Standard tier for autoscaling & performance |
| `mascan-sql` | Azure SQL Database | Southeast Asia | Production database (Standard S1) |
| `mascanstorage` | Storage Account | Southeast Asia | Blob storage for file uploads (LRS redundancy) |
| `Application Gateway` | Load Balancer | Southeast Asia | Distributes traffic across app instances |
| `Application Insights` | Monitoring | Southeast Asia | Telemetry & performance analytics |

### Data Flow
1. Users access the application through the Application Gateway
2. Traffic is load-balanced across two App Service instances
3. Attendance data is stored in Azure SQL Database
4. Uploaded files (CSV, PDF) are stored in Blob Storage
5. Application Insights monitors performance and errors

---

## 👥 Team & Credits

| Member | Role | Contributions |
|--------|------|---|
| **John Raymon Alba** | DevOps / Infrastructure | Resource Group setup, SQL Database, RBAC configuration, App Service deployment |
| **Fredrick Ortinero** | Cloud Architecture | App Service Plan, Storage Account, Database configuration, Autoscaling rules |
| **Divino Al Ricafort** | Application Deployment | GitHub Actions CI/CD, Environment setup, Application Insights, Blob Storage integration |
| **Mark Vincent Raña** | Documentation | Load Balancer setup, Monitoring configuration, Deployment documentation |

---

## ✨ Features

### Core Features
- ✅ **QR Code Generation**: Automatic QR code generation for quick attendance entry
- ✅ **Real-time Check-in**: Instant attendance recording with timestamp
- ✅ **Database Integration**: Persistent storage in Azure SQL Database
- ✅ **File Management**: CSV/PDF exports of attendance records via Blob Storage
- ✅ **Multi-instance Deployment**: Two app instances for reliability
- ✅ **Load Balancing**: Application Gateway distributes traffic
- ✅ **Auto-scaling**: Scales from 1 to 5 instances based on CPU load
- ✅ **Monitoring**: Application Insights telemetry and performance tracking
- ✅ **Security**: HTTPS, managed identities, proper firewall rules

### Technical Stack
- **Runtime**: Python 3.10+ with Flask framework
- **Database**: Azure SQL Database (Standard S1)
- **Storage**: Azure Blob Storage (Standard LRS)
- **Deployment**: GitHub Actions continuous deployment
- **Monitoring**: Application Insights with Python SDK
- **Load Balancing**: Application Gateway with health probes

---

## 🔧 Prerequisites

Before deployment, ensure you have:

- ✅ **Azure for Students Account** (free at [azure.microsoft.com/students](https://azure.microsoft.com/en-us/free/students/)) OR active Azure subscription
- ✅ **Azure CLI** (`az --version` to verify)
- ✅ **Docker Desktop** (for local testing)
- ✅ **Python 3.10+** (for local development)
- ✅ **Git** (for version control)
- ✅ **GitHub Account** (with repository access)

### Install Required Tools

**Azure CLI:**
```bash
# Windows
msiexec /i https://aka.ms/InstallAzureCliBundledWindows

# macOS
brew install azure-cli

# Linux
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

**Docker Desktop:**
- [Download for Windows/Mac](https://www.docker.com/products/docker-desktop/)
- Linux: `sudo apt-get install docker.io` (Ubuntu/Debian)

---

## 📘 Deployment Guide

For comprehensive step-by-step deployment instructions, see:

### 📄 [deployment/README.md](deployment/README.md)
Complete guide with 10 detailed steps covering:
1. Azure authentication
2. Resource Group creation
3. Container Registry setup
4. Docker image building & pushing
5. App Service Plan creation
6. Web App configuration
7. Environment variables
8. Persistent storage
9. Registry authentication
10. Application launch

**Estimated time**: 15-20 minutes

---

## 💰 Cost Estimation

### Monthly Breakdown (Azure for Students)

| Service | SKU | Cost | Notes |
|---------|-----|------|-------|
| App Service Plan | B2 Standard | ~$50 | Both instances included |
| Azure SQL Database | Standard S1 | ~$45 | 20 DTUs, scalable |
| Storage Account | Standard LRS | ~$5 | Blob storage for uploads |
| Application Gateway | Standard | ~$30 | Load balancing |
| Application Insights | Basic | ~$5 | Monitoring & telemetry |
| **Total** | | **~$135/month** | Covered by $100 student credits |

### Cost Optimization Tips
1. **Use Azure for Students**: Get $100 free monthly credits
2. **Enable Autoscaling**: Scale down to 1 instance during off-hours
3. **Reserved Instances**: Long-term commitment discounts
4. **Application Insights Sampling**: Reduce data ingestion costs

See [report/cost-estimate.md](report/cost-estimate.md) for detailed breakdown.

---

## 📚 Documentation

| Document | Purpose |
|----------|---------|
| [deployment/README.md](deployment/README.md) | Step-by-step deployment instructions |
| [README-AZURE (1).md](README-AZURE%20(1).md) | Original Azure deployment guide |
| [report/report.md](report/report.md) | Technical architecture report |
| [report/cost-estimate.md](report/cost-estimate.md) | Detailed cost analysis |
| [CHANGELOG.md](CHANGELOG.md) | Version history and changes |

---

## 🔍 Troubleshooting

### Application won't start
```bash
# Check logs
az webapp log tail --resource-group mascan-rg --name mascan-app

# Restart app
az webapp restart --resource-group mascan-rg --name mascan-app
```

### Database connection errors
```bash
# Verify firewall rules
az sql server firewall-rule list --resource-group mascan-rg --server mascan-server
```

### Performance issues
- Check Application Insights dashboard
- Monitor autoscaling metrics
- Review CPU/Memory usage in Azure Portal

---

## 🤝 Contributing

### Development Workflow
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/enhancement`)
3. Commit changes with descriptive messages
4. Push to branch (`git push origin feature/enhancement`)
5. Open a Pull Request with detailed description

### Code Standards
- Follow Flask best practices
- Use environment variables for configuration
- Add docstrings to functions
- Include unit tests for new features
- Update CHANGELOG.md with changes

### Reporting Issues
- Use GitHub Issues for bug reports
- Include steps to reproduce
- Attach error logs and screenshots
- Specify Python/Flask versions

---

## 📞 Support & Contact

For questions or issues:
- 📧 **Email**: Contact team members listed above
- 🎥 **Video Demo**: [YouTube Link](https://youtu.be/sHimFjR4bxU)
- 🌐 **Live App**: [MaScan Portal](https://mascan-qr-acexb0a8febaacab.southeastasia-01.azurewebsites.net/login)

---

## 📄 License

This project is part of CSEC 3 – Cloud Computing course.  
All rights reserved. © 2026 MaScan Team.

---

## 🎓 Educational Use

This repository demonstrates:
- Cloud architecture design and implementation
- Infrastructure-as-Code (IaC) principles
- CI/CD pipeline automation
- Scalability and high availability
- Cost optimization strategies
- Security best practices

**Perfect for**: Students learning Azure cloud deployment, DevOps practices, and cloud-native application development.


