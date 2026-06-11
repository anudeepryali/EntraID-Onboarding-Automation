# Entra ID Cloud Onboarding Automation

## Project Overview
This project is a production-ready Python automation tool designed for IT and System Administration teams. It solves a major administrative bottleneck: manual user provisioning in corporate cloud environments. 

The script ingests a standardized employee onboarding roster (`.csv`), validates the data, communicates securely with the **Microsoft Graph API**, and live-provisions enterprise accounts directly into **Microsoft Entra ID (formerly Azure Active Directory)** with randomized temporary credentials.

## Infrastructure & API Integration
* **API Framework:** Communicates directly with the Microsoft Graph REST API (`v1.0/users` endpoint).
* **Secure Authentication:** Implements OAuth 2.0 Client Credentials grant flow utilizing Entra ID App Registrations.
* **Production Security:** Zero hardcoded credentials. Designed to fetch sensitive variables (`Tenant ID`, `Client ID`, `Client Secret`) securely from system environment variables (`os.environ`).
* **Data Cleansing:** Standardizes naming conventions and automatically generates corporate-compliant User Principal Names (UPNs) and emails.

## System Architecture

The workflow below details how data flows securely from a local environment to the corporate Microsoft 365 cloud:

1. **Local Ingestion:** Script parses `new_hires.csv` and cleanses formatting.
2. **Authentication handshake:** Script sends Client Credentials to Microsoft Entra ID.
3. **Token Issuance:** Entra ID validates the App Registration permissions and returns an OAuth 2.0 Access Token.
4. **Cloud Provisioning:** Script sends a POST request with the user payload containing the token to the Microsoft Graph API, creating the live cloud user accounts.

## Deployment & Configuration

### 1. Azure App Registration Required Permissions
To run this script in an enterprise environment, an Azure App Registration must be created with the following **Application Permission** granted and consented to by a Global Administrator:
* `User.ReadWrite.All`

### 2. Environment Variables
To secure the automation pipeline, configure your runtime environment variables as follows:
* `AZURE_TENANT_ID`
* `AZURE_CLIENT_ID`
* `AZURE_CLIENT_SECRET`

### 3. Execution Data Sample (`new_hires.csv`)
```csv
FirstName,LastName,Department
Mark,Chandler,IT Support
Jack,Sparrow,Human Resources

**Plaintext**

Initializing Automated Cloud Onboarding System...
------------------------------------------------------------
[LIVE CLOUD] Successfully created Entra ID account for mchandler
[LIVE CLOUD] Successfully created Entra ID account for jsparrow
------------------------------------------------------------
Onboarding Complete.
