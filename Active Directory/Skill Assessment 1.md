![[../assets/Skill Assessment 1/image 204.png|image 204.png]]
  
uploaded rubeus and got the SPN for svc_sql
- used hashcat to get password
  
svc_sql:lucky7
  
ran ipconfig /all on the webshell powershell session to find the default gateway
- 10.129.0.1
  
ran fping to get all ips in the domain
```JavaScript
fping -asgq 10.129.0.0/16
```
- didn’t need this (i think its all htb academy vms)
  
pinging MS01 from the webshell shows that the IP is 172.16.6.50
![[../assets/Skill Assessment 1/image 1 151.png|image 1 151.png]]
  
uploaded an msfvenom reverse shell payload to get a meterpreter shell
  
execute powershell.exe in the reverse shell
  
establish a powershell session in MS01
![[../assets/Skill Assessment 1/image 2 135.png|image 2 135.png]]
  
  
not able to view any directories
![[../assets/Skill Assessment 1/image 3 121.png|image 3 121.png]]