# Server Side

# Check Syncronisation Settings
```
function check($user){
write-host $user -foregroundColor yellow
#Get-CASMailbox -Identity $user
#Get-Mailbox -Identity $user | Select-Object *Sync*, EmailAddressPolicyEnabled | ft
Get-CASMailbox -Identity $user | Select-Object ActiveSyncEnabled, OWAforDevicesEnabled
write-host "--------------------------"
}
```

### Check EmailAddressPolicySync
```
Get-Mailbox -Identity $user  | select EmailAddressPolicyEnabled
```
### Fix Server Side Sync
```
Set-Mailbox -Identity user@yourdomain.com -EmailAddressPolicyEnabled $true
```

### In Exchange Online PowerShell:
To enable server-side synchronization for the user, run this command:
```
Set-Mailbox -Identity user@yourdomain.com -EmailAddressPolicyEnabled $true
```

# Check if user has forwarding settings on mailbox
```
Get-Mailbox -Filter "ForwardingAddress -like '*'" | Select WindowsLiveID, *Forward*
```




# Other Commands


### Retrieves Client Access Settings for specified mailbox.
```
Get-CASMailbox -Identity $user
```
### Displays mailbox sync-related properties
```
Get-Mailbox -Identity $user | Select-Object *Sync* | ft
```
### Checks if ActiveSync and OWA for devices are enabled.
```
Get-CASMailbox -Identity $user | Select-Object ActiveSyncEnabled, OWAforDevicesEnabled
```
