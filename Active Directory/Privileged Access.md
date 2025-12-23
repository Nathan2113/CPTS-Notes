# Remote Access into Machines
## RDP
### PowerView
Enumerate members of the Remote Desktop Users group
```JavaScript
Get-NetLocalGroupMember -ComputerName <name> -GroupName "Remote Desktop Users"
```
![[../assets/Privileged Access/image 202.png|image 202.png]]
  
### Bloodhound
does the domain users group have CanRDP?
![[../assets/Privileged Access/image 1 149.png|image 1 149.png]]
  
if we have a user, do they have RDP privileges?
- under node info → execution rights
![[../assets/Privileged Access/image 2 133.png|image 2 133.png]]
- can also go under analysis → “Find Workstations where Domain Users have RDP” or “Find Servers where Domain Users can RDP”
    
    - xfreerdp, remina, or mstsc.exe
    
  
  
## WinRM
### PowerView
Enumerating the Remote Management Users Group
```JavaScript
Get-NetLocalGroupMember -ComputerName <name> -GroupName "Remote Management Users"
```
![[../assets/Privileged Access/image 3 119.png|image 3 119.png]]
  
### Bloodhound
can use custome cypher queries at the bottom of the screen
```JavaScript
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
```
![[../assets/Privileged Access/image 4 108.png|image 4 108.png]]
  
Can add the Cypher Quiery as a Custom Query to always have access to it
![[../assets/Privileged Access/image 5 105.png|image 5 105.png]]
  
  
### Establishing WinRM Session via Windows
```JavaScript
$password = ConvertTo-SecureString "<pass>" -AsPlainText -Force
$cred = new-object System.Management.Automation.PSCredential ("<DOMAIN>\<user>", $password)
Enter-PSSession -ComputerName <name> -Credential $cred
```
  
  
### Evil-WinRM
```JavaScript
evil-winrm -i <IP> -u <user> -p <pass>
OR
evil-winrm -i <IP> -u <user> -H <NT_hash>
```
  
## SQL Server Admin
where to find SQL creds
- Kerberoasting (most common)
- LLMNR Poisoning
- Snaffler (look at web.config)
  
Bloodhound custom cypher to find SQL Admins
```JavaScript
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2
```
![[../assets/Privileged Access/image 6 93.png|image 6 93.png]]
  
### Connect from Windows
PowerUpSQL has a command cheat sheet, and can be used to connect to the SQL instance
```JavaScript
Import-Module ./PowerUpSQL.ps1
Get-SQLInstanceDomain
```
![[../assets/Privileged Access/image 7 90.png|image 7 90.png]]
  
once you know the instance, you can connect to it and run queries
```JavaScript
Get-SQLQuery -Verbose -Instance "<IP>,<port>" -username "<DOMAIN>\<user>" -password "<pass>" -query 'Select @@version'
```
  
  
### Connect from Linux
```JavaScript
mssqlclient.py <DOMAIN>/<user>@<IP> -windows-auth
```
- enabling “xp_cmdshell” allows us to run system commands on the machine from the SQL instance
  
once xp_cmdshell is on, use the following to run commands
```JavaScript
xp_cmdshell <command>
```