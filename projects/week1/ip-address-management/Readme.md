## Deploy Windows Server

```sh
az vm image list \
  --publisher MicrosoftWindowsDesktop \
  --offer windows 11 \
  -sku win11-22h1-ent \
  --location eastus \
  --all \
  --output table
```

```sh
cd ip-address-management/templates/vm
az group create \
  --name net-fun-bootcamp \
  --location useast
az deployment group create \

  --resource-group myResourceGroup \
  --template-file template.bicep \
  --parameters @parameters.json  \
  --debug
```

