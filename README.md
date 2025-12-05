
# Cloud-Powered AI Workshop Guide

This repository contains the hands-on materials for the workshop **“Cloud-Powered Transformation: From Data to Smart Decisions with AI.”**  
Follow the steps below to prepare your Azure environment before running the notebooks.

© 2025 Kuncahyo Setyo Nugroho. All rights reserved.

---

For the workshop slide deck, access the file below: <a href="https://drive.google.com/file/d/1j_VHtO41w9m06XFIqj0PHXklD7jlC8tP/view?usp=sharing" target="_blank">
Click here to open the presentation
</a>

## 1. Preparation

### Create an Azure Student Account  
Get a free Azure Student subscription with **$100 credits**:  
https://azure.microsoft.com/en-us/free/students

### Log in to the Azure Portal  
https://portal.azure.com/auth/login/

### Check Remaining Credits  
1. In the Azure Portal, search for **Cost Management + Billing**  
2. Ensure your balance shows **$100.00 credits remaining**

### Network Requirements  
Use a stable internet connection during training and deployment.

---

## 2. Create Resource Group & Azure Machine Learning Workspace

### Create a Resource Group
1. Azure Portal → **Home**  
2. **Create a resource**  
3. Search: **Resource group**  
4. Click **Create**
5. **Region:** Select **Southeast Asia**  
   - This region must match your Azure Machine Learning workspace.

### Create Azure Machine Learning Workspace
1. Azure Portal → **Home**  
2. **Create a resource**
3. Search: **Machine Learning**
4. Click **Create Azure Machine Learning**
5. Select the same **Resource Group**
6. **Region:** Choose **Southeast Asia**  
7. Complete the workspace creation

> ⚠️ *Important Note:*  
> Resource Group and Azure ML Workspace **must be in the same region (Southeast Asia)** to avoid deployment issues.

---

## 3. Register Required Resource Providers

Open **Azure Cloud Shell (Bash)** from the top navigation bar in Azure Portal, then run:

```bash
# Core AML prerequisites
az provider register --namespace Microsoft.MachineLearningServices
az provider register --namespace Microsoft.Network
az provider register --namespace Microsoft.ContainerRegistry
az provider register --namespace Microsoft.KeyVault
az provider register --namespace Microsoft.ManagedIdentity
az provider register --namespace Microsoft.OperationalInsights
az provider register --namespace Microsoft.Insights

# Blockers commonly seen for managed online endpoints
az provider register --namespace Microsoft.PolicyInsights
az provider register --namespace Microsoft.Cdn

# Optional but often needed in end-to-end pipelines
az provider register --namespace Microsoft.Storage
az provider register --namespace Microsoft.Compute
az provider register --namespace Microsoft.Authorization
```
Registration may take several minutes.
## 4. Create a Compute Instance in Azure ML Studio

### Open Azure ML Studio  
Go to: https://ml.azure.com/

### Check Compute Quota  
Azure ML Studio → **Quota**  
Make sure your Student subscription still has available vCPU quota.

### Create a Compute Instance
1. In Azure ML Studio, open **Workspaces**  
2. Select your workspace (e.g., `bncc-ml`)
3. Go to **Manage → Compute**
4. Open the **Compute Instances** tab
5. Click **New** to create a new compute instance
6. Choose VM size:
   - **Standard_E4ds_v4** (Memory optimized, suitable for training)
7. Click **Next**
8. **Auto-shutdown:** Set to **OFF**  
   - The instance can be manually stopped when not in use
9. Click **Create**

### Launch JupyterLab
Once the compute instance is running:
- Open the instance → click **JupyterLab**

This is where all notebooks for training, scoring, and deployment will be executed.

## 5. Clone the Workshop Repository

Open the terminal inside your Azure ML compute instance or JupyterLab, then run:

```bash
git clone https://github.com/ksnugroho/bncc-ai-azure.git
```
### Important Files in the Repository:


### **```01-model-training.ipynb```**

Train a deep learning model for leaf disease classification.

**Includes:**
- dataset preparation  
- preprocessing and transforms  
- training EfficientNet  
- evaluation (accuracy, F1 score, confusion matrix, ROC)  
- saving the trained model to `artifacts/model.pth`

---

### **```02-model-scoring.ipynb```**

Create the `scoring.py` script used by Azure Machine Learning during inference.

**Includes:**
- generating the scoring script using `%%writefile`  
- testing the script offline using a base64-encoded image input  

---

### **```03-model-deployment.ipynb```**

Deploy the trained model to Azure ML as a **Managed Online Endpoint**.

**Includes:**
- registering the model  
- creating the environment (`conda.yaml`)  
- creating endpoint & deployment  
- updating traffic to the deployment  
- testing the scoring endpoint via REST API  

---

### **```04-gradio-inference.ipynb```**

Build a simple **Gradio** app that calls your deployed endpoint.  
Recommended to run **in Google Colab** so the Gradio app is publicly accessible.

**Includes:**
- capturing image input  
- converting image → base64  
- sending request to Azure endpoint  
- displaying predictions and confidence  

## 6. Clean Up Resources

At the end of the workshop, make sure to delete all Azure resources that were created.  
This prevents unnecessary credit usage on your Azure Student subscription.

### Option A — Delete the Entire Resource Group (Recommended)
This will automatically delete **all** resources inside it, including:
- Azure Machine Learning Workspace  
- Storage accounts  
- Container registries  
- Key vault  
- Any supporting resources created during deployment  

Steps:
1. Go to the Azure Portal → **Resource Groups**
2. Select your workshop resource group  
3. Click **Delete Resource Group**  
4. Type the resource group name to confirm  
5. Click **Delete**

This is the fastest and safest way to ensure no charges continue.

---

### Option B — Delete Resources Individually
If you prefer manual cleanup:
1. Delete the **Azure Machine Learning workspace**  
2. Delete the **Compute Instance** (make sure it is stopped first)  
3. Delete the **Online Endpoint**  
4. Delete associated Storage, Registry, or Key Vault (optional)

---

> ⚠️ *Important Note:*  
>Azure Student credits are limited.  
If resources are not deleted, credits may continue to decrease even when the services are idle , especially:
>- Compute instances left in "Running" state  
>- Managed online endpoints  
>- Storage accounts accumulating logs  
>Cleaning up ensures your credits remain available for future learning.
