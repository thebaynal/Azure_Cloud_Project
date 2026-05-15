# Changelog

All notable changes to this project will be documented in this file.

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
- `John Raymon Alba` - Created Resource Group 'mascan-rg' in Southeast Asia region with proper RBAC assignments
- `John Raymon Alba` - Set up Azure SQL Database server 'mascan-server' with Standard S1 pricing tier and configured firewall rules to allow authorized IPs
- `Fredrick Ortinero` - Created Azure Storage Account 'mascanstorage' with Standard LRS redundancy and blob container 'uploads' for CSV and PDF file storage
- `Fredrick Ortinero` - Created App Service Plan 'ASP-mascarqr' with B2 Standard tier to support autoscaling and multiple instances
- `Divino Al Ricafort` - Deployed first App Service instance 'mascan-qr' with Python 3.10 runtime and configured GitHub Actions continuous deployment
- `Divino Al Ricafort` - Configured environment variables in App Service: FLASK_ENV, DATABASE_URL, STORAGE_ACCOUNT_NAME, WEBSITES_PORT
- `Mark Vincent Raña` - Created second App Service instance 'mascan-qr-2' for fault tolerance and high availability on same App Service Plan
- `John Raymon Alba` - Set up Application Gateway with 2 instances for load balancing traffic between app services with health probe configuration
- `Fredrick Ortinero` - Configured autoscale rules: scale out when CPU > 70%, scale in when CPU < 30%, maximum 5 instances
- `Divino Al Ricafort` - Enabled Application Insights monitoring with Python SDK integration for telemetry collection
- `Mark Vincent Raña` - Created comprehensive deployment documentation with step-by-step Azure Portal instructions and screenshots

### Changed
- `John Raymon Alba` - Updated application requirements.txt to include pyodbc for Azure SQL Database driver support
- `Fredrick Ortinero` - Modified Flask database connection to use Azure SQL Database instead of SQLite with connection pooling
- `Divino Al Ricafort` - Updated file upload handler to use Azure Blob Storage SDK (azure-storage-blob) instead of local file system
- `Mark Vincent Raña` - Refactored environment variable loading to support Azure Key Vault (optional) and App Service configuration
- `John Raymon Alba` - Updated app startup script to migrate database schema on first deployment to Azure

### Fixed
- `Fredrick Ortinero` - Fixed database connection timeout issues by adjusting connection string parameters and SQL firewall rules
- `Divino Al Ricafort` - Resolved file upload path compatibility issues between Windows and Azure Linux containers
- `Mark Vincent Raña` - Corrected Application Gateway health probe endpoint configuration to match Flask application routes

### Removed
- `John Raymon Alba` - Removed hardcoded database path references to local SQLite database
- `Fredrick Ortinero` - Removed local file upload directory requirements (now uses Blob Storage)
- `Divino Al Ricafort` - Removed development-specific environment variables from production configuration

---

## [2026-05-11] - Project Initialization - Version 0.9

### Added
- `John Raymon Alba` - Created GitHub repository structure with diagram/, deployment/, and report/ folders
- `Fredrick Ortinero` - Designed baseline architecture diagram showing single App Service + SQL Database setup
- `Divino Al Ricafort` - Created architecture improvements diagram with autoscaling, load balancer, and monitoring components
- `Mark Vincent Raña` - Initialized project README.md with team member names and deployment instructions

### Changed
- `John Raymon Alba` - Updated main README.md to include Azure deployment guide and demo URL
- `Fredrick Ortinero` - Revised project structure to align with project requirements (diagram/, deployment/, report/ folders)


