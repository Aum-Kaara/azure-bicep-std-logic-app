Here’s a clean and professional **README.md** format for your **Logic App Standard Infrastructure Bicep deployment** — perfect for including in a GitHub repo 👇

---

# 🧱 Azure Logic App Standard Infrastructure — Bicep Deployment

This repository contains the **Infrastructure as Code (IaC)** setup for deploying an **Azure Logic App (Standard)** using **Bicep templates**.
It provisions all required components including the Logic App, App Service Plan, Storage Account, and Application Insights.

---

## 📁 Folder Structure

```
├── infra/
│   ├── logicapp-standard.bicep         # Main Bicep template
│   ├── logicapp.parameters.json        # Parameters file
│   └── README.md                       # This file
├── workflows/
│   └── sample-workflow.json            # Optional Logic App workflow definition
└── pipelines/
    └── azure-pipelines.yml             # Example CI/CD pipeline
```

---

## 🚀 Deployment Overview

The Bicep template automates the creation of:

* **App Service Plan** — Dedicated host for Logic App Standard
* **Storage Account** — Required for Logic App runtime state and workflow definitions
* **Application Insights** — For telemetry and monitoring
* **Logic App Standard** — The main integration runtime

---

## ⚙️ Prerequisites

Before deploying:

* Azure CLI installed (`az --version`)
* Bicep CLI installed (`az bicep version`)
* Access to an Azure subscription and resource group
* (Optional) Service connection if deploying via pipeline

---

## 🧩 Parameters

| Parameter            | Description                    | Example                                    |
| -------------------- | ------------------------------ | ------------------------------------------ |
| `logicAppName`       | Name of the Logic App Standard | `my-logicapp-standard`                     |
| `location`           | Azure region for deployment    | `eastus`                                   |
| `skuName`            | App Service Plan SKU           | `WS1`                                      |
| `storageAccountName` | Storage account name           | `mystandardlogicstorage`                   |
| `appInsightsName`    | Application Insights name      | `mylogicappinsights`                       |
| `tags`               | Tags for all resources         | `{ "env": "dev", "owner": "integration" }` |

---

## 🪜 Deployment Steps

### **1️⃣ Login to Azure**

```bash
az login
az account set --subscription "<your-subscription-id>"
```

### **2️⃣ Deploy Using Bicep**

```bash
az deployment group create \
  --name logicapp-deploy \
  --resource-group <your-resource-group> \
  --template-file infra/logicapp-standard.bicep \
  --parameters @infra/logicapp.parameters.json
```

### **3️⃣ Verify Deployment**

Check the Azure Portal → Your Resource Group
Ensure these resources exist:

* Logic App (Standard)
* App Service Plan
* Storage Account
* Application Insights

---

## 🧠 Optional: Deploy via Azure DevOps Pipeline

Example `pipelines/azure-pipelines.yml` snippet:

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: 'MyServiceConnection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      az deployment group create \
        --resource-group $(resourceGroupName) \
        --template-file infra/logicapp-standard.bicep \
        --parameters @infra/logicapp.parameters.json
```

---

## 🧾 Outputs

| Output        | Description                                          |
| ------------- | ---------------------------------------------------- |
| `logicAppUrl` | The default hostname (URL) of the Logic App Standard |

---

## 📈 Next Steps

After infrastructure deployment:

1. Add or import workflows under `/workflows`
2. Use Azure DevOps or GitHub Actions for CI/CD
3. Configure environment-specific parameter files (e.g., `dev.parameters.json`, `prod.parameters.json`)

---

## 🧰 Useful Commands

```bash
# Validate Bicep syntax
az bicep build --file infra/logicapp-standard.bicep

# Preview deployment changes
az deployment group what-if \
  --resource-group <rg-name> \
  --template-file infra/logicapp-standard.bicep \
  --parameters @infra/logicapp.parameters.json
```

---

## 🧹 Cleanup

To remove all deployed resources:

```bash
az group delete --name <your-resource-group> --yes --no-wait
```

---

## 📜 License

This project is licensed under the MIT License — you’re free to use, modify, and distribute it with attribution.

---

Would you like me to include a **`dev.parameters.json` / `prod.parameters.json`** example (for multiple environments)?
That makes the README more “enterprise-ready” for CI/CD use.
