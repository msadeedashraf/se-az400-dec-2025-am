
## To create a resource group

- Create a resource group using variables:
  
```
# Generate a random identifier
$randomIdentifier = Get-Random -Minimum 10000 -Maximum 99999

# Set variables
$location = "westus2"
$resourceGroup = "msdocs-rg-$randomIdentifier"

# Create the resource group
az group create `
  --name $resourceGroup `
  --location $location `
  --output json

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

$location = "westus"

# Put your existing RG name here (or set $resourceGroup = "msdocs-rg-xxxxx")

$resourceGroup = "msdocs-rg-29892"

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

# Clean up resources

```
# Get a list of resource groups in the active subscription
az group list --output table

# Delete a resource group and do not wait for the operation to finish
az group delete --name <msdocs-rg-0000000> --no-wait

```

## How to find the allowed regions

```
az policy assignment list --query "[].{name:name, displayName:displayName, scope:scope}" -o table

```

- To find the scope value
![scope](/Assets/to-get-scope.png)

- update the actual scope value from you cmd shell

```
$assignmentId = az policy assignment list --scope /subscriptions/77fd2e99-e14f-42c6-a054-bf277c8b683c --query "[?name=='sys.regionrestriction'].id | [0]" -o tsv
$assignmentId

```

- To see the allowed list of region in you subscription
  
```
az policy assignment list `
  --scope /subscriptions/77fd2e99-e14f-42c6-a054-bf277c8b683c `
  --query "[?name=='sys.regionrestriction'].parameters | [0]" `
  -o jsonc
```

- or 

```
# Common parameter name in many built-ins:
az policy assignment list --scope /subscriptions/77fd2e99-e14f-42c6-a054-bf277c8b683c --query "[?name=='sys.regionrestriction'].parameters.listOfAllowedLocations.value | [0]" -o jsonc

```