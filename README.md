# Azure tools

## Bastion Setup using SSH/RDP native client
[comment]: <> (23.05.2025)
<BR>

![image](https://github.com/user-attachments/assets/11cf17ea-0bf2-4b34-a5a3-fea557484a98)

### RDP
```
az network bastion rdp --name "<BastionName>" --resource-group "<RESOURCEGROUP>" --target-resource-id "/subscriptions/<SubscriptionID>/resourceGroups/<RESOURCEGROUP>/providers/Microsoft.Compute/virtualMachines/uservm1" --configure
```


- From MSTSC Display tab
    - Disable Full-screen mode (select resolution)
    - Disable "Use all my monitors for the remote session"

### SSH
- Add the public key to the VM
```
az network bastion ssh --name "<BastionName>" --resource-group "<RESOURCEGROUP>" --target-resource-id "/subscriptions/<SubscriptionID>/resourceGroups/<RESOURCEGROUP>/providers/Microsoft.Compute/virtualMachines/<VMNAME>" --auth-type "ssh-key" --username "azureuser" --ssh-key "/home/<USERNAME>/.ssh"
```

### Making alias to .zshrc / .bashrc
- Add the following lines to your .zshrc or .bashrc file
```
alias vm1='az network bastion rdp --name "<BastionName>" --resource-group "<RESOURCEGROUP>" --target-resource-id "/subscriptions/<SubscriptionID>/resourceGroups/<RESOURCEGROUP>/providers/Microsoft.Compute/virtualMachines/vm1" --configure'
```

**References:**
- https://learn.microsoft.com/en-us/azure/bastion/connect-vm-native-client-linux
- https://learn.microsoft.com/en-us/azure/bastion/connect-vm-native-client-windows
