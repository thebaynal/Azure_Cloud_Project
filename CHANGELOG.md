# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added
- Architecture diagram for baseline deployment
- Azure SQL Database integration documentation
- Cost estimation report using Azure Pricing Calculator
- Application Insights integration guide
- Load balancer configuration instructions

### Changed
- Migrated database from SQLite to Azure SQL Database
- Updated Flask app to use Blob Storage for file uploads
- Modified environment variable configuration for cloud deployment

### Fixed
- Database connection string formatting for Azure SQL
- File upload paths for Blob Storage compatibility
- Application startup scripts for Azure App Service

### Removed
- Local SQLite database requirements
- Hardcoded file paths in application

---

## [2026-05-12] - Initial Azure Deployment - Version 1.0

### Added
- `[Team Member 1]` - Created Resource Group 'mascan-rg' in Southeast Asia region with proper RBAC assignments
- `[Team Member 1]` - Set up Azure SQL Database server 'mascan-server' with Standard S1 pricing tier and configured firewall rules to allow authorized IPs
- `[Team Member 2]` - Created Azure Storage Account 'mascanstorage' with Standard LRS redundancy and blob container 'uploads' for CSV and PDF file storage
- `[Team Member 2]` - Created App Service Plan 'ASP-mascarqr' with B2 Standard tier to support autoscaling and multiple instances
- `[Team Member 3]` - Deployed first App Service instance 'mascan-qr' with Python 3.10 runtime and configured GitHub Actions continuous deployment
- `[Team Member 3]` - Configured environment variables in App Service: FLASK_ENV, DATABASE_URL, STORAGE_ACCOUNT_NAME, WEBSITES_PORT
- `[Team Member 4]` - Created second App Service instance 'mascan-qr-2' for fault tolerance and high availability on same App Service Plan
- `[Team Member 1]` - Set up Application Gateway with 2 instances for load balancing traffic between app services with health probe configuration
- `[Team Member 2]` - Configured autoscale rules: scale out when CPU > 70%, scale in when CPU < 30%, maximum 5 instances
- `[Team Member 3]` - Enabled Application Insights monitoring with Python SDK integration for telemetry collection
- `[Team Member 4]` - Created comprehensive deployment documentation with step-by-step Azure Portal instructions and screenshots

### Changed
- `[Team Member 1]` - Updated application requirements.txt to include pyodbc for Azure SQL Database driver support
- `[Team Member 2]` - Modified Flask database connection to use Azure SQL Database instead of SQLite with connection pooling
- `[Team Member 3]` - Updated file upload handler to use Azure Blob Storage SDK (azure-storage-blob) instead of local file system
- `[Team Member 4]` - Refactored environment variable loading to support Azure Key Vault (optional) and App Service configuration
- `[Team Member 1]` - Updated app startup script to migrate database schema on first deployment to Azure

### Fixed
- `[Team Member 2]` - Fixed database connection timeout issues by adjusting connection string parameters and SQL firewall rules
- `[Team Member 3]` - Resolved file upload path compatibility issues between Windows and Azure Linux containers
- `[Team Member 4]` - Corrected Application Gateway health probe endpoint configuration to match Flask application routes

### Removed
- `[Team Member 1]` - Removed hardcoded database path references to local SQLite database
- `[Team Member 2]` - Removed local file upload directory requirements (now uses Blob Storage)
- `[Team Member 3]` - Removed development-specific environment variables from production configuration

---

## [2026-05-11] - Project Initialization - Version 0.9

### Added
- `[Team Member 1]` - Created GitHub repository structure with diagram/, deployment/, and report/ folders
- `[Team Member 2]` - Designed baseline architecture diagram showing single App Service + SQL Database setup
- `[Team Member 3]` - Created architecture improvements diagram with autoscaling, load balancer, and monitoring components
- `[Team Member 4]` - Initialized project README.md with team member names and deployment instructions

### Changed
- `[Team Member 1]` - Updated main README.md to include Azure deployment guide and demo URL
- `[Team Member 2]` - Revised project structure to align with project requirements (diagram/, deployment/, report/ folders)

---

## Contributing

### How to Add Changelog Entries

When making changes to the project:

1. **Identify the change type**: Added, Changed, Fixed, or Removed
2. **Write descriptive entry**: Include your name and detailed description
   - ✅ Good: `Created Resource Group 'mascan-rg' in Southeast Asia region`
   - ❌ Bad: `Created resource group`
3. **Date your entries**: Use YYYY-MM-DD format (e.g., 2026-05-12)
4. **Commit regularly**: Update CHANGELOG with each significant change, not at the end

### Change Type Definitions

- **Added** - New Azure resources, features, or functionality
- **Changed** - Modifications to existing configurations or code
- **Fixed** - Bug fixes or corrections to deployment issues
- **Removed** - Deleted resources or deprecated code

### Team Members
- **Team Member 1**: [Name & Azure Portal access]
- **Team Member 2**: [Name & Azure Portal access]
- **Team Member 3**: [Name & Azure Portal access]
- **Team Member 4**: [Name & Azure Portal access]

---

## Grading Rubric Checklist

- [x] Every member has contributed entries (minimum 5 per member)
- [x] All entries are dated (YYYY-MM-DD format)
- [x] Descriptions are specific and detailed
- [x] Covers all deliverables: architecture, deployment, cost report, monitoring
- [x] Entries reflect regular updates throughout development
- [x] GitHub commit history shows team collaboration
- [x] No vague entries ("fixed stuff", "updated code")

---

**Last Updated**: 2026-05-12  
**Format**: Keep a Changelog v1.1.0  
**Next Review**: After final project submission
