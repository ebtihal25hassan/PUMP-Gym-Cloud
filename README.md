# PUMP-Gym
# PUMP Gym OS - Cloud Infrastructure 🚀

> A fully implemented, 100% free-tier, and highly secured Cloud Infrastructure  layer for **PUMP Gym OS**, aligned with the Master Technical Specification.

a gym management platform made of one responsive Flutter app (User and Admin interfaces), an ASP.NET Core backend and a SQL Server database.

---

# Goal

Run the whole cloud side of PUMP on Azure at **zero cost**, using only free tiers and free offers, without changing the API contracts, DTOs, enums or database schema

---

## 🏗️ Provisioned Azure Architecture (What is Done)

The cloud infrastructure has been fully provisioned and secured using **Azure Free/Serverless Tiers**. The environment is production-ready for the development team to deploy their code.

### 1. Hosted Services
* **`pump-web` (Azure Static Web Apps):** Hosts the Flutter Web application for both User and Admin interfaces. (SKU: Free).
* **`pump-api` (Azure App Service):** Hosts the ASP.NET Core Web API. (SKU: F1 Free).
* **`pump-db` (Azure SQL Database):** Serverless SQL database (Gen5, 2 vCores). Overage billing is strictly **Disabled** (Auto-pauses to save compute).

### 2. Supporting Cloud Services
* **`pumpblobstorage` (Azure Blob Storage):** Used for storing user avatars and exported gym reports. Access tier is set to `Hot`, with `LRS` redundancy to eliminate unnecessary costs.
* **`pump-signalr` (Azure SignalR Service):** Enables real-time WebSocket communication for live gym occupancy features. (Tier: Free).
* **`pump-insights` (Application Insights):** Application monitoring and error logging. Protected by a `0.1 GB` daily cap to ensure zero costs.

### 3. Security & Secrets Management (Zero-Trust Approach)
* **`pump-key` / `pump-vault` (Azure Key Vault):** Centralized secret management. 
* **Managed Identity:** The `pump-api` uses System-Assigned Managed Identity (`Key Vault Secrets User` role) to fetch connection strings at runtime without embedding any credentials in the source code or `appsettings.json`.

---

## 🛠️ Environment Variables Configuration (Key Vault References)

The API is securely configured using `@Microsoft.KeyVault` references. The following settings are successfully injected into the App Service:
- `ConnectionStrings__Default`
- `ConnectionStrings__BlobStorage`
- `Azure__SignalR__ConnectionString`
- `ApplicationInsights__ConnectionString`
- `Jwt__Key`
- `Cors__AllowedOrigins__0` -> `https://gentle-bush-08396d20f.2.azurestaticapps.net`

---

## ⚠️ Errors Encountered & Troubleshooting (Lessons Learned)

During the cloud setup, we encountered and resolved several architectural challenges:

### 1. F1 Free Tier Quota Limit Error (`image_7372b2.png`)
* **Error:** `Operation cannot be completed without additional quota. Current Limit (F1 VMs): 0`.
* **Cause:** Azure restricts subscriptions to only *one* Free (F1) App Service Plan per region (or per subscription depending on the account type). 
* **Fix:** Ensured no hidden/orphaned F1 plans existed in the same region, and correctly deployed `pump-api` within the allowed quota constraints without upgrading to a paid tier.

### 2. Storage Account Redundancy Costs
* **Issue:** Azure defaults Blob Storage to Geo-Redundant Storage (RA-GRS), which incurs costs.
* **Fix:** Explicitly downgraded redundancy to Locally Redundant Storage (`LRS`) and disabled versioning/soft-delete to maintain a $0 footprint for avatar storage.

### 3. SCM Basic Auth & Deployment Setup
* **Issue:** Disabling SCM Basic Auth prevents Visual Studio from deploying via `.PublishSettings`.
* **Fix:** Re-enabled `SCM Basic Auth` temporarily for seamless CI/CD handover to the backend developer, confirming it has no billing impact.

---

## 🚦 Project Status & Next Steps

### ✅ Completed:
- [x] Azure Cloud Architecture design and cost review ($0 footprint achieved).
- [x] Provisioned all resources (SQL, App Service, Static Web App, Key Vault, Storage, SignalR).
- [x] Configured Managed Identities, CORS, and Networking Firewalls.
- [x] Secret management via Key Vault References (Green Checkmarks verified).
- [x] Downloaded Publish Profile and prepared the Handover Document.
- [ ] Set up Azure DevOps YAML Pipelines (CI/CD) for automated deployments .
