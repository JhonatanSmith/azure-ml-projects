# Azure Machine Learning Projects 🚀

This repository showcases multiple real-world machine learning projects built using **Azure Machine Learning Studio** and the **Python SDK v2**. These projects aim to demonstrate not only cloud-based ML development workflows, but also how to structure scalable solutions using best practices from the industry.

> ⚙️ All workspaces, resources, and deployments are created programmatically using the Azure ML SDK v2 — from zero to hero.

---

## 🔧 Project Structure

Each project lives in its own folder, with an independent and clean setup.

```yml
smith-azure-ml-projects/  (Resource Group)
│
├── ml-azure-genai/       (Workspace: Churn Analysis + GenAI)
│   ├── Tags:
│   │   ├── env: dev
│   │   └── project: churn-genai
│   ├── Data Assets
│   ├── Compute Cluster
│   ├── Experiments
│   ├── Registered Models
│   └── Deployed Endpoints
│
├── ml-client-segmentation/  (Coming soon)
├── ml-revenue-forecast/     (Planned)
```
## 📁 Project Index

- [Churn Analysis with GenAI](churn-analysis+genai/)
- [Client Segmentation with Azure ML](client-segmentation-analysis+genai/)

# Initial Setup (from your terminal)

Of course, every good project will need an environment to run properly. To do so, we will require installing some libraries. Be sure to run the following code in **the root** folder as a very first step (when you write code from zero to hero). Of course, this will run in the terminal. And of course, activate it!

```bash
# Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Install dependencies
pip install azure-ai-ml azure-identity
pip install azure-cli

# Log in to Azure
az login

```

# 🗃️ Configuration File

**We will work with a `config.json` file to store all the information we need to work with the Azure ML platform. This file will be created in the root folder of the project.** Just simply create a JSON file that will store all the information we need to work with the Azure ML platform.

```json
{
  "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",  ←  obtained with `az account show`
  "resource_group": "your-resource-group-name",                 ← created by code with Azure SDK   
  "workspace_name": "work-space-name",                          ← created by code with Azure SDK
  "location": "eastus"                                          ← a valid region
}
```

``We could use the Azure CLI client to get the subscription_id, resource_group, and workspace_name.``

After we create the resource group, we will need to create a workspace. To do so, we will use the Azure CLI.

```bash
# to get subscription_id
az account show --query id -o tsv

# To create a resource group if not exists
az group create --name smith-azure-ml-projects --location eastus
```

# 🧠 Creating the Azure ML Workspace (via SDK)

This will create a workspace with the name **ml-churn-genai** in the resource group **smith-azure-ml-projects** in the location **eastus**. This could also be done with the Azure Python SDK.

```python
from azure.identity import DefaultAzureCredential
from azure.ai.ml import MLClient
from azure.ai.ml.entities import Workspace
import json

with open("config.json") as f:
    config = json.load(f)

cred = DefaultAzureCredential()

ml_client = MLClient(
    credential=cred,
    subscription_id=config["subscription_id"],
    resource_group_name=config["resource_group"]
)

workspace = Workspace(
    name=config["workspace_name"],
    location=config["location"],
    display_name="Azure ML Projects :P",
    description="Workspace to host multiple ML experiments and GenAI prototypes",
    tags={"env": "dev", "project": "churn-genai"}
)

created_ws = ml_client.workspaces.begin_create(workspace).result()
print(f"✅ Workspace '{created_ws.name}' created successfully.")

```

## Creating the Azure ML Workspace (via CLI) 

This is optional but is another way to create the workspace via the CLI.

```bash
az ml workspace create \
  --name ml-azure-genai \
  --resource-group smith-azure-ml-projects \
  --location eastus

```

# ✅ Ready? Let’s start building!

Once your workspace is live, head to the notebooks/ folder to start developing your first use case: Churn Analysis + GenAI-based explanations.