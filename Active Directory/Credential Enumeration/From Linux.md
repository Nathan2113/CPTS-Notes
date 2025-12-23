# Crackmapexec
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --users
```
![image 354.png](/image%20354.png)
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --groups
```
![image 1 262.png](/image%201%20262.png)
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --loggedon-users
```
![image 2 225.png](../../assets/From%20Linux/image%202%20225.png)
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --shares
```
![image 3 195.png](../../assets/From%20Linux/image%203%20195.png)
  
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> -M spider_plus --share <share_name>
```
![image 4 171.png](../../assets/From%20Linux/image%204%20171.png)
- will dig through all readable files in the share
- will write results to JSON file stored in /tmp/cme_spider_plus/<ip>
![image 5 160.png](../../assets/From%20Linux/image%205%20160.png)
  
  
# SMBMap
```JavaScript
smbmap -u <user> -p <pass> -d <DOMAIN> -H <IP>
```
![image 6 139.png](../../assets/From%20Linux/image%206%20139.png)
  
to recursively list all directories in a share
```JavaScript
smbmap -u <user> -p <pass> -d <DOMAIN> -H <IP> -R <share> --dir-only
```
![image 7 130.png](../../assets/From%20Linux/image%207%20130.png)
  
  
# RPCClient
```JavaScript
rpcclient -U "" -N <IP>
```
  
## RPC Enumeration
```JavaScript
queryuser 0x<RID>
```
![image 8 114.png](../../assets/From%20Linux/image%208%20114.png)
  
```JavaScript
enumdomusers
```
![image 9 105.png](../../assets/From%20Linux/image%209%20105.png)
  
  
# Impacket
## Psexec.py
- uploads a randomly-named executable to the ADMIN$ share → registers the service via RPC and Windows Service Control Manager
    
    - once established, communication happens over a named pipe
    
- need local administrator privileges
```JavaScript
impacket-psexec <DOMAIN>/<user>:'<pass>'@<IP>
```
![image 10 89.png](../../assets/From%20Linux/image%2010%2089.png)
  
## Wmiexec.py
- executes commands through Windows Management Instrumentation
    
    - does not drop any files onto host and generates fewer logs
    
- runs as the local administrator
    
    - harder to detect than running commands as SYSTEM
    
```JavaScript
wmiexec.py <DOMAIN>/<user>:'<pass>'@<IP>
```
![image 11 81.png](../../assets/From%20Linux/image%2011%2081.png)
  
  
## Windapsearch
- enumerate users, groups, and computers by utilizing LDAP queries
  
Getting domain admins
```JavaScript
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```
![image 12 69.png](../../assets/From%20Linux/image%2012%2069.png)
  
Getting privileged users
```JavaScript
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```
![image 13 69.png](../../assets/From%20Linux/image%2013%2069.png)