
## To create a resource group

- Create a resource group using variables:
  
```
$randomIdentifier = (Get-Random) * (Get-Random)
$location = "eastus"
$resourceGroup = "msdocs-rg-$randomIdentifier"

az group create --name $resourceGroup --location $location --output json
```

- To show the created resource

```
az group list --output table

az group show --name $resourceGroup --output table

az resource list --resource-group $resourceGroup --output table
```

## Create a storage account

# -----------------------------
# Variables
# -----------------------------

# Create a random numeric suffix (good for uniqueness + valid SA name)

```
$randomIdentifier = Get-Random -Minimum 100000 -Maximum 999999

$location = "westus2"

# Put your existing RG name here (or set $resourceGroup = "msdocs-rg-xxxxx")
$resourceGroup = "msdocs-rg-0000000"

# Storage account name rules: lowercase letters and numbers only, 3-24 chars
$storageAccount = "msdocssa$randomIdentifier"

# -----------------------------
# Create the storage account
# -----------------------------
Write-Host "Creating storage account $storageAccount in resource group $resourceGroup"

az storage account create `
  --name $storageAccount `
  --resource-group $resourceGroup `
  --location $location `
  --sku Standard_RAGRS `
  --kind StorageV2 `
  --output json

  
```