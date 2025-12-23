Our client asked us to perform testing from their machine, but we don’t have outside internet access and are not able to import any tools, so we can only use tools/commands native to Active Directory
- can also be a more stealthy approach
  
# Env Commands For Host and Network Recon
## Basic Enumeration Commands
![image 200.png](../assets/Living%20Off%20the%20Land/image%20200.png)
  
## Systeminfo
running one command like this will generate fewer logs
  
# Harnessing PowerShell
![image 1 148.png](../assets/Living%20Off%20the%20Land/image%201%20148.png)
  
![image 2 132.png](../assets/Living%20Off%20the%20Land/image%202%20132.png)
  
![image 3 118.png](../assets/Living%20Off%20the%20Land/image%203%20118.png)
  
  
## Downgrade PowerShell
powershell event logging was introduced in powershell 3.0
- since Windows computers often have multiple versions of powershell, and defenders often don’t defend against these other versions, we can attempt to call Powershell version 2.0 or older, preventing our commands from being logged in Event Viewer
  
check current version
```JavaScript
Get-host
```
![image 4 107.png](../assets/Living%20Off%20the%20Land/image%204%20107.png)
  
call older version
```JavaScript
powershell.exe -version 2
Get-host
```
![image 5 104.png](../assets/Living%20Off%20the%20Land/image%205%20104.png)
  
## Examining Powershell Event Logs
need to check if we are still creating logs
can check the following locations
- Applications and Services Logs → Microsoft → Windows → PowerShell → Operational → PowerShell Operational Log
- Application and Services Logs → Windows PowerShell
![image 6 92.png](../assets/Living%20Off%20the%20Land/image%206%2092.png)
  
  
# Checking Defenses
  
## Firewall Checks
```JavaScript
netsh advfirewall show allprofiles
```
![image 7 89.png](../assets/Living%20Off%20the%20Land/image%207%2089.png)
  
```JavaScript
sc query windefend
```
![image 8 80.png](../assets/Living%20Off%20the%20Land/image%208%2080.png)
  
show AV settings
```JavaScript
Get-MpComputerStatus
```
![image 9 77.png](../assets/Living%20Off%20the%20Land/image%209%2077.png)
  
  
# Am I Alone?
need to see if anyone else is logged into the computer with you
- if they are, there’s a chance they can notice what you are doing
    
    - for example, they get a popup from something you run
    
  
```JavaScript
qwinsta
```
![image 10 66.png](../assets/Living%20Off%20the%20Land/image%2010%2066.png)
  
  
# Network Information
![image 11 61.png](../assets/Living%20Off%20the%20Land/image%2011%2061.png)
  
```JavaScript
arp -a
```
![image 12 50.png](../assets/Living%20Off%20the%20Land/image%2012%2050.png)
  
```JavaScript
route print
```
![image 13 50.png](../assets/Living%20Off%20the%20Land/image%2013%2050.png)
  
  
# Windows Management Instrumentation (WMI)
scripting engine to retrieve information and run administrative tasks
- we us it to report on domain users, groups, processes, etc
  
![image 14 46.png](../assets/Living%20Off%20the%20Land/image%2014%2046.png)
  
  
```JavaScript
wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
```
![image 15 42.png](../assets/Living%20Off%20the%20Land/image%2015%2042.png)
  
  
# Net Commands
net commands are often logged, but you can try using “net1” instead of “net” to potentially get around triggering logs from the net string
```JavaScript
net1 user
```
![image 16 37.png](../assets/Living%20Off%20the%20Land/image%2016%2037.png)
  
query local host and remote hosts, can list the following
- local and domain users
- groups
- hosts
- specific users in groups
- domain controllers
- password requirements
![image 17 31.png](../assets/Living%20Off%20the%20Land/image%2017%2031.png)
  
## Listing Domain Groups
```JavaScript
net group /domain
```
![image 18 29.png](../assets/Living%20Off%20the%20Land/image%2018%2029.png)
  
## Information about a Domain User
```JavaScript
net user /domain <username>
```
![image 19 26.png](../assets/Living%20Off%20the%20Land/image%2019%2026.png)
  
  
# Dsquery
queries we run with this tools can easily be recreated with BloodHound and PowerView, but in the event that we can’t use those tools we have dsquery
  
dsquery will exist on any host with the “Active Directory Domain Services Role” installed
- C:\Windows\System32\dsquery.dll
  
## User Search
```JavaScript
dsquery user
```
![image 20 22.png](../assets/Living%20Off%20the%20Land/image%2020%2022.png)
  
```JavaScript
dsquery computer
```
![image 21 22.png](../assets/Living%20Off%20the%20Land/image%2021%2022.png)
  
can use a dsquery wildcard search to view all objects in the OU
```JavaScript
dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```
![image 22 19.png](../assets/Living%20Off%20the%20Land/image%2022%2019.png)
  
  
## Dsquery with LDAP
can combine dsquery wth LDAP search filters
- an example is looking for useres with “PASSWD_NOTREQD” flag in the “userAccountControl” attribute
```JavaScript
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
```
![image 23 16.png](../assets/Living%20Off%20the%20Land/image%2023%2016.png)
  
### Searching for Domain Controllers
```JavaScript
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
![image 24 13.png](../assets/Living%20Off%20the%20Land/image%2024%2013.png)
  
### LDAP Filtering Explained
“userAccountControl:1.2.840.113556.1.4.803:” specifies that we are looking for the UAC attributes for an object
- at the end fo the string, we put =<decimal_value> where the decimal value is what UAC attribute we want to search for
![image 25 9.png](../assets/Living%20Off%20the%20Land/image%2025%209.png)
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