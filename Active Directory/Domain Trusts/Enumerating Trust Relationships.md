# Cmdlet
Use Get-ADTrust cmdlet to enumerate trust relationships
```JavaScript
Import-Module activedirectory
Get-ADTrust -Filter *
```
![[../../assets/Enumerating Trust Relationships/image 379.png|image 379.png]]
  
# PowerView
## Get-DomainTrust
```JavaScript
Get-DomainTrust 
```
![[../../assets/Enumerating Trust Relationships/image 1 283.png|image 1 283.png]]
  
## Get-DomainTrustMapping
```JavaScript
Get-DomainTrustMapping
```
![[../../assets/Enumerating Trust Relationships/image 2 241.png|image 2 241.png]]
  
## Checking Users in the Child Domain Using Get-DomainUser
```JavaScript
Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```
![[../../assets/Enumerating Trust Relationships/image 3 206.png|image 3 206.png]]
  
  
# Using Netdom
## Query Domain Trust
```JavaScript
netdom query /domain:<DOMAIN> trust
```
![[../../assets/Enumerating Trust Relationships/image 4 181.png|image 4 181.png]]
  
## Using Netdom to Query Domain Controllers
```JavaScript
netdom query /domain:<DOMAIN> dc
```
![[../../assets/Enumerating Trust Relationships/image 5 169.png|image 5 169.png]]
  
## Using Netdom to Query Workstations and Servers
```JavaScript
netdom query /domain:<DOMAIN> workstation
```
![[../../assets/Enumerating Trust Relationships/image 6 144.png|image 6 144.png]]
  
# BloodHound
Pre-built query in Analysis tab
![[../../assets/Enumerating Trust Relationships/image 7 135.png|image 7 135.png]]