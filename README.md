# Azure CLI Security Evaluation Scripts

## Overview
This repository contains an extensive set of Azure CLI scripts designed for security engineers to audit and secure their Azure environments. The scripts target various areas including network security, identity management, resource compliance, and monitoring.

## Prerequisites
Before running the scripts, ensure you have the following:
- Azure CLI installed ([Installation Guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)).
- Appropriate permissions to access the resources and services in your Azure environment.
- An active Azure account.

## Usage
To use any of the scripts, simply copy the relevant command from the list below and run it in your terminal with Azure CLI installed. You may need to replace specific values such as resource group names, virtual network names, or vault names with your environment's values.

## List of Scripts

1. **List All Azure Resources**
   ```bash
   az resource list --output table
   ```

2. **Check Network Security Groups (NSG) Rules**
   ```bash
   az network nsg list --output table
   ```

3. **List All User Roles and Permissions**
   ```bash
   az role assignment list --all --output table
   ```

4. **Identify Unused or Inactive Accounts**
   ```bash
   az ad user list --filter "accountEnabled eq false"
   ```

5. **Audit Key Vault Access**
   ```bash
   az keyvault list --query "[].{Name:name, AccessPolicies:accessPolicies}" --output table
   ```

6. **Check Diagnostic Settings**
   ```bash
   az monitor diagnostic-settings list --output table
   ```

7. **Review Azure Security Center Recommendations**
   ```bash
   az security task list --output table
   ```

8. **Evaluate Azure Policies**
   ```bash
   az policy state summarize --output table
   ```

9. **Monitor Failed Login Attempts**
   ```bash
   az monitor activity-log list --max-events 50 --query "[?operationName.value == 'Microsoft.Authorization/login/action' && status.value == 'Failed']" --output table
   ```

10. **Check for Expiring or Expired Certificates**
   ```bash
   az keyvault certificate list --vault-name <YourVaultName> --query "[?attributes.expires<='2024-08-25']" --output table
   ```

11. **Audit for Publicly Accessible Storage Accounts**
   ```bash
   az storage account list --query "[?enableHttpsTrafficOnly==false]" --output table
   ```

12. **Check for Virtual Machines Without Disk Encryption**
   ```bash
   az vm list --query "[?storageProfile.osDisk.encryptionSettings.enabled==false]" --output table
   ```

13. **Identify Expired Azure Active Directory Tokens**
   ```bash
   az ad token list --query "[?status == 'Expired']" --output table
   ```

14. **Inspect Azure Subnets for Route Tables and User Defined Routes**
   ```bash
   az network vnet subnet list --vnet-name <vnet-name> --resource-group <resource-group-name> --query "[].{Name:name, RouteTable:routeTable.id, UserDefinedRoutes:ipConfigurations}" --output table
   ```

15. **Audit Idle Public IP Addresses**
   ```bash
   az network public-ip list --query "[?ipAddress != null && ipConfiguration == null]" --output table
   ```

16. **List VMs with Open Management Ports (SSH, RDP)**
   ```bash
   az network nsg rule list --query "[?contains(destinationPortRange, '22') || contains(destinationPortRange, '3389')]" --output table
   ```

17. **Check for SQL Servers with Public Network Access Enabled**
   ```bash
   az sql server list --query "[?publicNetworkAccess=='Enabled']" --output table
   ```

18. **Audit Network Watchers**
   ```bash
   az network watcher list --output table
   ```

19. **List Azure Activity Logs**
   ```bash
   az monitor activity-log list --output table
   ```

20. **Check for Azure Security Center Compliance States**
   ```bash
   az security assessment list --query "[].{ResourceId:resourceId, Status:status.code}" --output table
   ```

21. **Identify Publicly Exposed Web Applications**
   ```bash
   az webapp list --query "[?enabledHostNames[0].contains('azurewebsites.net')]" --output table
   ```

22. **Evaluate Azure SQL Databases for Encryption**
   ```bash
   az sql db list --query "[?transparentDataEncryption[0].status != 'Enabled']" --output table
   ```

23. **List All Azure Subscriptions and Their Policies**
   ```bash
   az account list --output table
   az policy state summarize --management-group <YourManagementGroup> --output table
   ```

24. **Audit Azure AD Password Policies**
   ```bash
   az ad user list --query "[].{User:userPrincipalName, PasswordPolicies:passwordPolicies}" --output table
   ```

25. **Monitor Storage Account Security Posture**
   ```bash
   az storage

 account list --query "[?encryption.services.blob.enabled != 'true' || enableHttpsTrafficOnly != 'true']" --output table
   ```

26. **Check for Insecure Load Balancers**
   ```bash
   az network lb list --query "[?frontendIPConfigurations[0].privateIPAddress == null]" --output table
   ```

27. **Evaluate Identity and Access Management (IAM) Role Changes**
   ```bash
   az monitor activity-log list --query "[?operationName.value == 'Microsoft.Authorization/roleAssignments/write']" --output table
   ```

28. **Monitor Azure Kubernetes Service (AKS) Cluster Security**
   ```bash
   az aks list --query "[].{ClusterName:name, RBACEnabled:enableRBAC, NetworkPolicy:networkProfile.networkPolicy}" --output table
   ```

29. **Audit Virtual Machine Extensions**
   ```bash
   az vm extension list --query "[?provisioningState == 'Failed']" --output table
   ```

30. **Check for Unused Managed Disks**
   ```bash
   az disk list --query "[?managedBy == null]" --output table
   ```

31. **Evaluate Azure Function Security**
   ```bash
   az functionapp list --query "[?hostNames[0].contains('azurewebsites.net') && enabledHostNames[0].contains('https')]" --output table
   ```

32. **Audit ExpressRoute Circuits**
   ```bash
   az network express-route list --query "[].{Name:name, State:provisioningState, PeeringType:peerings[0].peeringType}" --output table
   ```

## Contribution
Feel free to open issues or submit pull requests if you have improvements or suggestions.

## License
This project is licensed under the MIT License.
