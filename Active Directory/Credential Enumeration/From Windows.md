# ActiveDirectory PowerShell Module
- Get-Module
    
    - lists all available modules, including custom administrator scripts
    
![[../../assets/From Windows/image 359.png|image 359.png]]
```JavaScript
Import-Module ActiveDirectory
```
- if you don’t see ActiveDirectory Module
![[../../assets/From Windows/image 1 267.png|image 1 267.png]]
  
## Get Domain Info
```JavaScript
Get-ADDomain
```
![[../../assets/From Windows/image 2 226.png|image 2 226.png]]
  
## Get-ADUser
can list all users withe ServicePrincipleName populated
- can show who is susceptible to kerberoasting
```JavaScript
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
![[../../assets/From Windows/image 3 196.png|image 3 196.png]]
  
## Checking for Trust Relationships
```JavaScript
Get-ADTrust -Filter *
```
![[../../assets/From Windows/image 4 172.png|image 4 172.png]]
- useful for child-to-parent trust relationship attacks
  
## Group Enumeration
```JavaScript
Get-ADGroup -Filter * | select name
```
![[../../assets/From Windows/image 5 161.png|image 5 161.png]]
  
## Detailed Group Info
```JavaScript
Get-ADGroup -Identity "Backup Operators"
```
![[../../assets/From Windows/image 6 140.png|image 6 140.png]]
- now that we know more about the group, we can get group members
```JavaScript
Get-ADGroupMember -Identity "Backup Operators"
```
![[../../assets/From Windows/image 7 131.png|image 7 131.png]]
- can see that the user “backupagent” is a backup operator
    
    - if we can take over this account, we can compromise the domain
    
  
  
# Powerview
used to enumerate misconfigurations in the domain
- much like bloodhound, but can help us find more subtle configurations
![[../../assets/From Windows/image 8 115.png|image 8 115.png]]
![[../../assets/From Windows/image 9 106.png|image 9 106.png]]
  
## Domain User Information
```JavaScript
Get-DomainUser -Identity <user> -Domain <DOMAIN> | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```
![[../../assets/From Windows/image 10 90.png|image 10 90.png]]
  
## Recursive Group Membership
adding the -Recurse flag tells PowerView that if it finds any groups that are part of the target group (nested groups), to list out the members of those groups as well
- if we find a SecAdmins group that is part of the domain admins, and we use “Get-DomainGroupMember” with -Recurse, it will list out the members of SecAdmins as well
```JavaScript
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```
![[../../assets/From Windows/image 11 82.png|image 11 82.png]]
  
## Trust Enumeration
```JavaScript
Get-DomainTrustMapping
```
![[../../assets/From Windows/image 12 70.png|image 12 70.png]]
  
## Testing for Local Admin Access
```JavaScript
Test-AdminAccess -ComputerName <computer_name>
```
![[../../assets/From Windows/image 13 70.png|image 13 70.png]]
- can perform that same action for each host to see where our user has admin access
  
## Finding Users with SPN Set
```JavaScript
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```
![[../../assets/From Windows/image 14 64.png|image 14 64.png]]
  
  
# SharpView
Can be used if the client has hardened against powershell, or we need to avoid using powershell
.Net port of PowerView
- has many of the same functions
  
can see the help page for all methods used in powerview with the following
```JavaScript
.\SharpView.exe <function> -Help
```
![[../../assets/From Windows/image 15 59.png|image 15 59.png]]
  
can use SharpView to enumerate user information
```JavaScript
.\SharpView.exe Get-DomainUser -Identity <user>
```
![[../../assets/From Windows/image 16 53.png|image 16 53.png]]
  
  
# Snaffler
helps find sensitive data in an environment
- lists all hosts in the domain, then enumerates those hosts for shares and readable directories
  
```JavaScript
Snaffler.exe -s -d <DOMAIN> -o snaffler.log -v data
```
- -s → print to console
- -d → domain
- -o → write results to file
- -v → verbose
    
    - data → displays results to screen
    
![[../../assets/From Windows/image 17 47.png|image 17 47.png]]
- uses same color scheme as Group3r
  
# SharpHound
same process as BloodHound, but done from a Windows host
```JavaScript
.\SharpHound.exe -c All --zipfilename <zip_name>
```
![[../../assets/From Windows/image 18 45.png|image 18 45.png]]
  
important query for enumeration (in analysis tab)
```JavaScript
Find Computers with Unsupported Operating Systems
```
- finds outdated and unsupported systems using legacy software
    
    - can give us a path to using exploits like MS08-067
    
  
```JavaScript
Find Computers where Domain Users are Local Admin
```
- can be used to dump more credentials and move laterally