flaw in MS-RPRN (Print System Remote Protocol)
- defines communication of print job processing and print system management between client and server
  
To leverage this flaw, any domain user can connect to the spool’s named pipe with “RpcOpenPrinter” and use “RpcRemoteFirstPrinterChangeNotificationEx” and force the server to authenticate to any host over SMB
  
spooler system runs as SYSTEM and is installed by default on Windows servers running Desktop Experience
- can be leveraged to relay LDAP and give attacker DC Sync
- can also be used to relay LDAP auth and grant RBCD, giving the attacker privileges to authenticate as any user on the victim’s computer
  
can be used to compromise the DC on a partner domain/forest if you have administrative access to a DC in the first domain/forest, and allows TGT delegation (not default anymore)
  
Use “Get-SpoolStatus” to check for machines vulnerable to “MS-PRN Printer Bug”
- can be used toe compromise a host in another forest that has Unconstrained Delegation enabled
  
# Enumerate for Printer Bug
```JavaScript
Import-Module .\SecurityAssessment.ps1
Get-SpoolStatus -ComputerName <DC_name>.<DOMAIN>
```
![image 370.png](../../assets/Printer%20Bug/image%20370.png)