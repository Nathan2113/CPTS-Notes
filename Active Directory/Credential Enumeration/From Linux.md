# Crackmapexec
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --users
```
![[/image 354.png|image 354.png]]
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --groups
```
![[/image 1 262.png|image 1 262.png]]
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --loggedon-users
```
![[../../assets/From Linux/image 2 225.png|image 2 225.png]]
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --shares
```
![[../../assets/From Linux/image 3 195.png|image 3 195.png]]
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> -M spider_plus --share <share_name>
```
![[../../assets/From Linux/image 4 171.png|image 4 171.png]]
- will dig through all readable files in the share
- will write results to JSON file stored in /tmp/cme_spider_plus/<ip>
![[../../assets/From Linux/image 5 160.png|image 5 160.png]]
  
  
# SMBMap
```JavaScript
smbmap -u <user> -p <pass> -d <DOMAIN> -H <IP>
```
![[../../assets/From Linux/image 6 139.png|image 6 139.png]]
  
to recursively list all directories in a share
```JavaScript
smbmap -u <user> -p <pass> -d <DOMAIN> -H <IP> -R <share> --dir-only
```
![[../../assets/From Linux/image 7 130.png|image 7 130.png]]
  
  
# RPCClient
```JavaScript
rpcclient -U "" -N <IP>
```
  
## RPC Enumeration
```JavaScript
queryuser 0x<RID>
```
![[../../assets/From Linux/image 8 114.png|image 8 114.png]]
  
```JavaScript
enumdomusers
```
![[../../assets/From Linux/image 9 105.png|image 9 105.png]]
  
  
# Impacket
## Psexec.py
- uploads a randomly-named executable to the ADMIN$ share → registers the service via RPC and Windows Service Control Manager
    
    - once established, communication happens over a named pipe
    
- need local administrator privileges
```JavaScript
impacket-psexec <DOMAIN>/<user>:'<pass>'@<IP>
```
![[../../assets/From Linux/image 10 89.png|image 10 89.png]]
  
## Wmiexec.py
- executes commands through Windows Management Instrumentation
    
    - does not drop any files onto host and generates fewer logs
    
- runs as the local administrator
    
    - harder to detect than running commands as SYSTEM
    
```JavaScript
wmiexec.py <DOMAIN>/<user>:'<pass>'@<IP>
```
![[../../assets/From Linux/image 11 81.png|image 11 81.png]]
  
  
## Windapsearch
- enumerate users, groups, and computers by utilizing LDAP queries
  
Getting domain admins
```JavaScript
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```
![[../../assets/From Linux/image 12 69.png|image 12 69.png]]
  
Getting privileged users
```JavaScript
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```
![[../../assets/From Linux/image 13 69.png|image 13 69.png]]