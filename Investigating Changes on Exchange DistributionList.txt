# Exchange Online PowerShell Guide  

## Prerequisites  
- Install **.NET 4.8** to avoid random errors.  

## Installation  
Run the following commands in PowerShell:  
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12  
Install-PackageProvider -Name NuGet  
Install-Module -Name ExchangeOnlineManagement -Force -AllowClobber  
Import-Module ExchangeOnlineManagement  
```
---

## Common Error and Fix  

### Error:  
```
An error occurred while sending the request.
At C:\ProgramFiles\WiineManagement\3.6.ineManagement.psml:762 char:21
throw $Exception.InnerException; + CategoryInfo: OperationStopped: (: ) [], HttpRequestException
+ FullyQualifiedErrorId: An error occurred while sending the request.
```
### Fix:  
Run the following command to resolve the issue:  
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12  
```

---

## Connect to Exchange Online (Microsoft 365)  
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12  
Connect-ExchangeOnline -UserPrincipalName <adm_user> -ShowProgress $true  
```

Alternatively, a simpler way to connect:  
```powershell
Connect-ExchangeOnline -UserPrincipalName <admin_user>
```

---

## Check If Logging Is Enabled  
Run this command to check if auditing is enabled:  
```powershell
Get-AdminAuditLogConfig  
```  
Enable auditing if it's not already enabled:  
```powershell
Set-AdminAuditLogConfig -AuditEnabled $true  
```

---

## Searching for Logs  

### 1️⃣ Find the **ObjectId** of the Distribution List  
Run the following command:  
```powershell
Get-DistributionGroup -Identity "shareinbox@domain.com"  
```
#### Sample Output (Redacted):  
```
WindowsEmailAddress: shareinbox@necsws.com  
Identity: Shareinbox20250103170428  
Id: Shareinbox20250103170428  
```
The **ObjectId** is: `Shareinbox20250103170428`

### 2️⃣ Search Unified Audit Logs  
Run the following command to retrieve logs for the Distribution List:  
```powershell
Search-UnifiedAuditLog -StartDate (Get-Date).AddDays(-365) -EndDate (Get-Date) -ObjectIds "Shareinbox20250103170428"
```

---

## Other Useful Commands  

### Get Members of a Distribution Group  
```powershell
Get-DistributionGroupMember -Identity "shareinbox@domain.com"  
```

### Search Logs for Specific Operations  
```powershell
Search-UnifiedAuditLog -StartDate "2025-02-01" -EndDate "2025-02-20" -Operations "AddMemberToGroup", "RemoveMemberFromGroup" -ObjectIds "Marketing-Team"  
```


🔗 **More Info:** [Microsoft Audit Log Activities](https://learn.microsoft.com/en-us/purview/audit-log-activities)  
