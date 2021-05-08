# AzureAD Join Windows Server 2019

1.  Create new Az VM with a system assigned managed identity with image of either 2019 LTSC or Windows 10 1809.

```bash
az vm create --resource-group <rg> --name <vmname> --image Win2019Datacenter --assign-identity --admin-username <user> --admin-password <password>
```

2. Install the Microsoft.Azure.ActiveDirectory extension on the VM:

```bash
az vm extension set --publisher Microsoft.Azure.ActiveDirectory --name AADLoginForWindows --vm-name <vmname> --resource-group <rg>
```

3. Assign either the Virtual Machine Administrator Login or the Virtual Machine User Login role assignment:

```powershell
New-AzRoleAssignment -Scope <resourceID> -ObjectId <objectref> -RoleDefinitionName 'Virtual Machine Administrator Login'
# or
New-AzRoleAssignment -Scope <resourceID> -ObjectId <objectref> -RoleDefinitionName 'Virtual Machine User Login' 
```
```bash
# or
az role assignment create --role "Virtual Machine Administrator Login" --assignee-object-id --scope <resourceID>
```

4. Add user into either BUILTIN\Administrators or Remote Desktop Users:s

```powershell
Add-LocalGroupMember -Group Administrators -Member <member>
# or
Add-LocalGroupMember -Group 'Remote Desktop Users' -Member <member>
```