# Crackmapexec
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --pass-pol
```
![[../../assets/Enumerating Password Policy/image 356.png|image 356.png]]
  
# RPCClient
```JavaScript
rpcclient -U "" -N <IP>
querydominfo
getdompwinfo
```
![[../../assets/Enumerating Password Policy/image 1 264.png|image 1 264.png]]
- “querydominfo” gives domain info
![[../../assets/Enumerating Password Policy/image 2 223.png|image 2 223.png]]
- “getdompwinfo” gives domain’s password policy
  
# Enum4linux
```JavaScript
enum4linux -P <IP>
```
![[../../assets/Enumerating Password Policy/image 3 193.png|image 3 193.png]]
  
# enum4linux-ng
- rewrite of enum4linux in python
- has additional features
    
    - can export data as YAML or JSON
    
```JavaScript
enum4linux-ng -P <IP> -oA <filename>
```
![[../../assets/Enumerating Password Policy/image 4 169.png|image 4 169.png]]
![[../../assets/Enumerating Password Policy/image 5 158.png|image 5 158.png]]
  
# LDAP
LDAP enumeration tools
- windapsearch.py
- ldapsearch
- ad-ldapsdomaindump.py
```JavaScript
ldapsearch -H <IP> -x -b "DC=<DOMAIN>,DC=<DOMAIN>" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
![[../../assets/Enumerating Password Policy/image 6 137.png|image 6 137.png]]
- in older versions, -h is used instead of -H
  
  
# From Windows
```JavaScript
net use \\DC01\ipc$ "" /u:""
```
- establishes a null session
- can also use valid creds
  
```JavaScript
net accounts
```
![[../../assets/Enumerating Password Policy/image 7 129.png|image 7 129.png]]
gives us the following good information
- passwords never expire
- minimum length is 8 so weak passwords are likely in use
- lockout threshold is 5 wrong passwords
- account remains locked out for 30 minutes
- we can use this info above to do the following
    
    - try common passwords like “Welcome1”
    
    - attempt 2-3 times (to be safe)
    
    - can spray every 31 minutes
        
        - if account gets locked, it’ll automatically unlock after 30 minutes
        
    
  
```JavaScript
import-module ./PowerView.ps1
Get-DomainPolicy
```
- using powerview to get pasword policy
![[../../assets/Enumerating Password Policy/image 8 113.png|image 8 113.png]]
- same output as “net accounts”, but tell us that PasswordComplexity=1
    
    - means that a password must have 3/4 of the following
        
        - uppercase
        
        - lowercase
        
        - number
        
        - special character
        
    
    - weak passwords like “Welcome1” would still satisfy this rule
    
  
![[../../assets/Enumerating Password Policy/image 9 104.png|image 9 104.png]]
- default domain policy for passwords