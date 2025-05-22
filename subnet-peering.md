# Configure subnet peerings
- Ref. https://learn.microsoft.com/en-us/azure/virtual-network/how-to-configure-subnet-peering
```
az network vnet peering create --name VNET-1_to_VNET-2 \
                               --resource-group <RESOURCEGROUP> \
                               --vnet-name VNET-1 \
                               --remote-vnet VNET-2 \
                               --allow-forwarded-traffic \
                               --allow-gateway-transit \
                               --allow-vnet-access \
                               --peer-complete-vnet false \
                               --local-subnet-names default aadds-subnet \
                               --remote-subnet-names default aadds-subnet

az network vnet peering create --name VNET-2_to_VNET-1 \
                               --resource-group <RESOURCEGROUP> \
                               --vnet-name VNET-2 \
                               --remote-vnet VNET-1 \
                               --allow-forwarded-traffic \
                               --allow-gateway-transit \
                               --allow-vnet-access \
                               --peer-complete-vnet false \
                               --local-subnet-names default aadds-subnet \
                               --remote-subnet-names default aadds-subnet

## Add new
az network vnet peering update --name VNET-1_to_VNET-2 \
                               --resource-group <RESOURCEGROUP> \
                               --vnet-name VNET-1 \
                               --local-subnet-names default aadds-subnet \
                               --remote-subnet-names default aadds-subnet

az network vnet peering update --name VNET-2_to_VNET-1 \
                               --resource-group <RESOURCEGROUP> \
                               --vnet-name VNET-2 \
                               --remote-subnet-names default aadds-subnet
                           
## Sync peerings
az network vnet peering sync --name VNET-1_to_VNET-2 \
                             --resource-group <RESOURCEGROUP> \
                             --vnet-name VNET-1

az network vnet peering sync --name VNET-2_to_VNET-1 \
                             --resource-group <RESOURCEGROUP> \
                             --vnet-name VNET-2

## Show peerings
az network vnet peering show --name VNET-1_to_VNET-2  \
                             --resource-group <RESOURCEGROUP> \
                             --vnet-name VNET-1 -o table

az network vnet peering show --name VNET-2_to_VNET-1 \
                             --resource-group <RESOURCEGROUP> \
                             --vnet-name VNET-2 -o table

## View effective routes
az network nic show-effective-route-table \
  --name VMnicXYZ \
  --resource-group <RESOURCEGROUP> -o table

az network nic show-effective-route-table \
  --name aadds-XYZ-nic \
  --resource-group <RESOURCEGROUP> -o table
  
## Delete peering
az network vnet peering delete --name VNET-1_to_VNET-2 \
                               --resource-group <RESOURCEGROUP> \
                               --vnet-name VNET-1

az network vnet peering delete --name VNET-2_to_VNET-1 \
                               --resource-group <RESOURCEGROUP> \
                               --vnet-name VNET-2

## Create a needs NSG inbound rule for Entra DS. CorpNetSaw Service tag not in Azure Portal.
az network nsg rule create --name AllowRD_Created \
                           --resource-group <RESOURCEGROUP> \
                           --nsg-name aadds-nsg \
                           --priority 201 \
                           --access Allow \
                           --direction Inbound \
                           --protocol TCP \
                           --source-address-prefix CorpNetSaw \
                           --source-port-range "*" \
                           --destination-address-prefix "*" \
                           --destination-port-range 3389
