If this is set for a user, they are not subject to the domain’s password policy
- they can make a shorter password or no password at all
  
just because this flag is set, doesn’t mean the user doesn’t have a password, just that it’s not required
  
PowerView
```JavaScript
Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol
```
![[../../assets/PASSWD_NOTREQD Field/image 373.png|image 373.png]]