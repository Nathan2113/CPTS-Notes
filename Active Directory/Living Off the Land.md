Our client asked us to perform testing from their machine, but we don’t have outside internet access and are not able to import any tools, so we can only use tools/commands native to Active Directory
- can also be a more stealthy approach
  
# Env Commands For Host and Network Recon
## Basic Enumeration Commands
![[../assets/Living Off the Land/image 200.png|image 200.png]]
  
## Systeminfo
running one command like this will generate fewer logs
  
# Harnessing PowerShell
![[../assets/Living Off the Land/image 1 148.png|image 1 148.png]]
  
![[../assets/Living Off the Land/image 2 132.png|image 2 132.png]]
  
![[../assets/Living Off the Land/image 3 118.png|image 3 118.png]]
  
  
## Downgrade PowerShell
powershell event logging was introduced in powershell 3.0
- since Windows computers often have multiple versions of powershell, and defenders often don’t defend against these other versions, we can attempt to call Powershell version 2.0 or older, preventing our commands from being logged in Event Viewer
  
check current version
```JavaScript
Get-host
```
![[../assets/Living Off the Land/image 4 107.png|image 4 107.png]]
  
call older version
```JavaScript
powershell.exe -version 2
Get-host
```
![[../assets/Living Off the Land/image 5 104.png|image 5 104.png]]
  
## Examining Powershell Event Logs
need to check if we are still creating logs
can check the following locations
- Applications and Services Logs → Microsoft → Windows → PowerShell → Operational → PowerShell Operational Log
- Application and Services Logs → Windows PowerShell
![[../assets/Living Off the Land/image 6 92.png|image 6 92.png]]
  
  
# Checking Defenses
  
## Firewall Checks
```JavaScript
netsh advfirewall show allprofiles
```
![[../assets/Living Off the Land/image 7 89.png|image 7 89.png]]
  
```JavaScript
sc query windefend
```
![[../assets/Living Off the Land/image 8 80.png|image 8 80.png]]
  
show AV settings
```JavaScript
Get-MpComputerStatus
```
![[../assets/Living Off the Land/image 9 77.png|image 9 77.png]]
  
  
# Am I Alone?
need to see if anyone else is logged into the computer with you
- if they are, there’s a chance they can notice what you are doing
    
    - for example, they get a popup from something you run
    
  
```JavaScript
qwinsta
```
![[../assets/Living Off the Land/image 10 66.png|image 10 66.png]]
  
  
# Network Information
![[../assets/Living Off the Land/image 11 61.png|image 11 61.png]]
  
```JavaScript
arp -a
```
![[../assets/Living Off the Land/image 12 50.png|image 12 50.png]]
  
```JavaScript
route print
```
![[../assets/Living Off the Land/image 13 50.png|image 13 50.png]]
  
  
# Windows Management Instrumentation (WMI)
scripting engine to retrieve information and run administrative tasks
- we us it to report on domain users, groups, processes, etc
  
![[../assets/Living Off the Land/image 14 46.png|image 14 46.png]]
  
  
```JavaScript
wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
```
![[../assets/Living Off the Land/image 15 42.png|image 15 42.png]]
  
  
# Net Commands
net commands are often logged, but you can try using “net1” instead of “net” to potentially get around triggering logs from the net string
```JavaScript
net1 user
```
![[../assets/Living Off the Land/image 16 37.png|image 16 37.png]]
  
query local host and remote hosts, can list the following
- local and domain users
- groups
- hosts
- specific users in groups
- domain controllers
- password requirements
![[../assets/Living Off the Land/image 17 31.png|image 17 31.png]]
  
## Listing Domain Groups
```JavaScript
net group /domain
```
![[../assets/Living Off the Land/image 18 29.png|image 18 29.png]]
  
## Information about a Domain User
```JavaScript
net user /domain <username>
```
![[../assets/Living Off the Land/image 19 26.png|image 19 26.png]]
  
  
# Dsquery
queries we run with this tools can easily be recreated with BloodHound and PowerView, but in the event that we can’t use those tools we have dsquery
  
dsquery will exist on any host with the “Active Directory Domain Services Role” installed
- C:\Windows\System32\dsquery.dll
  
## User Search
```JavaScript
dsquery user
```
![[../assets/Living Off the Land/image 20 22.png|image 20 22.png]]
  
```JavaScript
dsquery computer
```
![[../assets/Living Off the Land/image 21 22.png|image 21 22.png]]
  
can use a dsquery wildcard search to view all objects in the OU
```JavaScript
dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```
![[../assets/Living Off the Land/image 22 19.png|image 22 19.png]]
  
  
## Dsquery with LDAP
can combine dsquery wth LDAP search filters
- an example is looking for useres with “PASSWD_NOTREQD” flag in the “userAccountControl” attribute
```JavaScript
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
```
![[../assets/Living Off the Land/image 23 16.png|image 23 16.png]]
  
### Searching for Domain Controllers
```JavaScript
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
![[../assets/Living Off the Land/image 24 13.png|image 24 13.png]]
  
### LDAP Filtering Explained
“userAccountControl:1.2.840.113556.1.4.803:” specifies that we are looking for the UAC attributes for an object
- at the end fo the string, we put =<decimal_value> where the decimal value is what UAC attribute we want to search for
![[../assets/Living Off the Land/image 25 9.png|image 25 9.png]]
- for example, checking “Password Can’t Change” would be userAccountControl:1.2.840.113556.1.4.803:=64
  
### Object Identifier (OID) Match Strings
the last number before *=<decimal_value>
- gives the rules for string matching, and they are as follows
  
1. 1.2.840.113556.1.4.803
    
    1. bit value must match exactly - great for matching a singular attribute
    
1. 1.2.840.113556.1.4.804
    
    1. show any attribute match if any bit in the chain matches
    
1. 1.2.840.113556.1.4.1941
    
    1. search through all ownership and membership entries
    
  
### Logical Operators
```JavaScript
(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=64))
```
- must be a user and has “Password Can’t Change”
  
```JavaScript
(&(objectClass=user)(!userAccountControl:1.2.840.113556.1.4.803:=64))
```
- must be a user that does NOT have “Password Can’t Change”