# Cmdlet
Use Get-ADTrust cmdlet to enumerate trust relationships
```JavaScript
Import-Module activedirectory
Get-ADTrust -Filter *
```
![image 379.png](../../assets/Enumerating%20Trust%20Relationships/image%20379.png)
  
# PowerView
## Get-DomainTrust
```JavaScript
Get-DomainTrust 
```
![image 1 283.png](../../assets/Enumerating%20Trust%20Relationships/image%201%20283.png)
  
## Get-DomainTrustMapping
```JavaScript
Get-DomainTrustMapping
```
![image 2 241.png](../../assets/Enumerating%20Trust%20Relationships/image%202%20241.png)
  
## Checking Users in the Child Domain Using Get-DomainUser
```JavaScript
Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```
![image 3 206.png](../../assets/Enumerating%20Trust%20Relationships/image%203%20206.png)
  
  
# Using Netdom
## Query Domain Trust
```JavaScript
netdom query /domain:<DOMAIN> trust
```
![image 4 181.png](../../assets/Enumerating%20Trust%20Relationships/image%204%20181.png)
  
## Using Netdom to Query Domain Controllers
```JavaScript
netdom query /domain:<DOMAIN> dc
```
![image 5 169.png](../../assets/Enumerating%20Trust%20Relationships/image%205%20169.png)
  
## Using Netdom to Query Workstations and Servers
```JavaScript
netdom query /domain:<DOMAIN> workstation
```
![image 6 144.png](../../assets/Enumerating%20Trust%20Relationships/image%206%20144.png)
  
# BloodHound
Pre-built query in Analysis tab
![image 7 135.png](../../assets/Enumerating%20Trust%20Relationships/image%207%20135.png)