# AZ-400 Hands-on Case Studies: Cloud Development in Azure

These case studies are designed to give you hands-on experience with common Azure services: **Azure SQL Database**, **Azure Storage**, **Web Apps**, **Azure Functions**, and **Virtual Machines**. They are suitable for students using the Azure for Students portal.

---

## Case Study 1: Deploy an Azure SQL Database for a Web Application

### 🎯 Objective
Create an Azure SQL Database to store product data for a sample web application.

### 📝 Assumptions
- You have access to an Azure for Students subscription.
- Azure CLI is installed on your machine.

### ✅ Steps

1. **Login to Azure CLI**
    ```bash
    az login
    ```

2. **Create a Resource Group**
    ```bash
    az group create --name sql-rg --location eastus
    ```

3. **Create a SQL Server**
    ```bash
    az sql server create \
      --name sample-sql-server \
      --resource-group sql-rg \
      --location eastus \
      --admin-user azureuser \
      --admin-password Pa55w.rd123
    ```

4. **Create SQL Database**
    ```bash
    az sql db create \
      --resource-group sql-rg \
      --server sample-sql-server \
      --name sampledb \
      --service-objective S0
    ```

5. **Allow Azure Services to Access the Server**
    ```bash
    az sql server firewall-rule create \
      --resource-group sql-rg \
      --server sample-sql-server \
      --name AllowYourIP \
      --start-ip-address 0.0.0.0 \
      --end-ip-address 0.0.0.0
    ```

---

## Case Study 2: Upload and Access Files Using Azure Blob Storage

### 🎯 Objective
Create a storage account and use Blob Storage to store and access user images.

### ✅ Steps

1. **Create a Storage Account**
    ```bash
    az storage account create \
      --name mystorageaccount12345 \
      --resource-group sql-rg \
      --location eastus \
      --sku Standard_LRS
    ```

2. **Create a Blob Container**
    ```bash
    az storage container create \
      --name userimages \
      --account-name mystorageaccount12345 \
      --public-access blob
    ```

3. **Upload a File**
    ```bash
    az storage blob upload \
      --account-name mystorageaccount12345 \
      --container-name userimages \
      --name sampleimage.jpg \
      --file ./sampleimage.jpg
    ```

---

## Case Study 3: Host a Web Application using Azure Web App

### 🎯 Objective
Deploy a Python Flask application to Azure Web App.

### ✅ Steps

1. **Create an App Service Plan**
    ```bash
    az appservice plan create \
      --name myAppServicePlan \
      --resource-group sql-rg \
      --sku FREE
    ```

2. **Create a Web App**
    ```bash
    az webapp create \
      --resource-group sql-rg \
      --plan myAppServicePlan \
      --name myflaskapp12345 \
      --runtime "PYTHON|3.9"
    ```

3. **Enable Deployment (Local Git)**
    ```bash
    az webapp deployment source config-local-git \
      --name myflaskapp12345 \
      --resource-group sql-rg
    ```

4. **Push Code (from local git repo)**
    - Git remote is shown in the command above.
    - Push using `git push azure master` after committing your code.

---

## Case Study 4: Create and Trigger an Azure Function

### 🎯 Objective
Create a Python HTTP-triggered Azure Function that logs requests.

### ✅ Steps

1. **Create a Function App**
    ```bash
    az functionapp create \
      --resource-group sql-rg \
      --consumption-plan-location eastus \
      --runtime python \
      --functions-version 4 \
      --name myfunctionapp12345 \
      --storage-account mystorageaccount12345
    ```

2. **Deploy Your Function**
    - Use Azure Functions Core Tools (`func init`, `func new`, `func azure functionapp publish myfunctionapp12345`)
    - Or deploy using VS Code with the Azure extension.

3. **Test the Function**
    - Use the Function App URL in your browser or tools like Postman to send test HTTP requests.

---

## Case Study 5: Create and Connect to an Azure Virtual Machine

### 🎯 Objective
Provision a Linux VM to test shell scripts and deployment automation.

### ✅ Steps

1. **Create the VM**
    ```bash
    az vm create \
      --resource-group sql-rg \
      --name TestVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

2. **Open SSH Port**
    ```bash
    az vm open-port \
      --port 22 \
      --resource-group sql-rg \
      --name TestVM
    ```

3. **Get Public IP and SSH into the VM**
    ```bash
    az vm list-ip-addresses \
      --name TestVM \
      --resource-group sql-rg \
      -o table
    ```

    Use the IP to SSH:
    ```bash
    ssh azureuser@<public-ip>
    ```

---

## 📝 Notes for Students

- Replace sample names (e.g., `mystorageaccount12345`) with unique names to avoid conflicts.
- Delete resources after practice to avoid using up Azure student quotas:
    ```bash
    az group delete --name sql-rg --no-wait --yes
    ```

---

**Happy Building on Azure! 🚀**
