# Azure CLI Security Evaluation Scripts

## Overview
This repository contains a set of Azure CLI scripts designed for security engineers to quickly evaluate and audit the security posture of their Azure environment. These scripts can help you identify potential security risks, enforce best practices, and ensure that your environment is secure.

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

## Contribution
Feel free to open issues or submit pull requests if you have improvements or suggestions.

## License
This project is licensed under the MIT License.
