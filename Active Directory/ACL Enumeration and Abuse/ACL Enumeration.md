# PowerView
## Using Find-InterestingDomainAcl
```JavaScript
Find-InterestingDomainAcl
```
![image 363.png](../../assets/ACL%20Enumeration/image%20363.png)
  
## Using Get-DomainObjectACL
Can perform target enumeration over users to see what ACLs they have
```JavaScript
Import-Module ./PowerView.ps1
$sid = Conver-NameToSid <user>
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
![image 1 271.png](../../assets/ACL%20Enumeration/image%201%20271.png)
- can see that the user has an ACL with GUID “00299570-246d-11d0-a768-00aa006e0529”
    
    - googling shows that the ACL is “**User-Force-Change-Password”**
    
  
can also do a reverse search in PowerShell to get the ACE name
```JavaScript
$guid= "<guid>"
Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```
![image 2 230.png](../../assets/ACL%20Enumeration/image%202%20230.png)
  
### Using the -ResolveGUIDs Flag
The method shown above is inefficient, but we can have PowerView to do this for us
```JavaScript
Import-Module ./PowerView.ps1
$sid = Convert-NameToSid <user>
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
![image 3 199.png](../../assets/ACL%20Enumeration/image%203%20199.png)
  
## Creating a List of Domain Users and Grabbing ACLs
```JavaScript
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
foreach($line in [System.IO.File]::ReadLines("/path/to/ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match '<DOMAIN\\<user>'}}
```
![image 4 175.png](../../assets/ACL%20Enumeration/image%204%20175.png)
- check which users in AD our user has permissions over
  
Checking if a group is nested in another (they inherit all rights from the parent)
```JavaScript
Get-DomainGroup -Identity "<group>" | select memberof
```
![image 5 164.png](../../assets/ACL%20Enumeration/image%205%20164.png)
  
we got wley from LLMNR poisoning → he has GenericWrite over damudsen → damundsen can add people to “Help Desk Level 1” → “Help Desk Level 1” inherits from “Information Technology”
- now we want to see what the “Information Technology” group can do
  
```JavaScript
$itgroupsid = Convert-NameToSid "<group>"
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $itgroupsid} -Verbose
```
![image 6 142.png](../../assets/ACL%20Enumeration/image%206%20142.png)
- GenericAll over adunn
  
now see what adunn can do using same tactics as above
![image 7 133.png](../../assets/ACL%20Enumeration/image%207%20133.png)
  
# Bloodhound
I know everything they talked about already
- check bloodhound notes or notes on individual ACL abuses