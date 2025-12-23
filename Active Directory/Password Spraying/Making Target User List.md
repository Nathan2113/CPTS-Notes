# Detailed User Enumeration
SMB Null Session
  
LDAP anonymous bind
  
Kerbrute
- use statistically-likely-usernames GitHub repo or linkedin2username
  
Use credentials given by client
  
LLMNR/NBT-NS response
  
# Always Log Activities
- accounts targeted
- domain controller used in attack
- time of the spray
- date of the spray
- password(s) attempted
  
# Already on an Internal Computer
if we are on an internal machine and have SYSTEM, we can impersonate the computer account
# SMB Null Session
```JavaScript
enum4linux -U <IP> | grep "user:" | cute -f2 -d"[" | cut -f1 -d"]"
```
![[../../assets/Making Target User List/image 357.png|image 357.png]]
  
# RPCClient
```JavaScript
rpcclient -U "" -N <IP>
enumdomusers
```
![[../../assets/Making Target User List/image 1 265.png|image 1 265.png]]
  
# Crackmapexec
```JavaScript
crackmapexec smb <IP> --users
```
![[../../assets/Making Target User List/image 2 224.png|image 2 224.png]]
  
  
# LDAP Anonymous
```JavaScript
ldapsearch -h <IP> -x -b "DC=<DOMAIN>,DC=<DOMAIN>" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```
  
## Windapsearch
- make ldap searching easier (should still understand ldap search filters
```JavaScript
./windapsearch.py --dc-ip <IP> -u "" -U
```
![[../../assets/Making Target User List/image 3 194.png|image 3 194.png]]
  
# Kerbrute
```JavaScript
kerbrute userenum -d <DOMAIN> --dc <IP> /path/to/wordlist
```
- good wordlist is jsmith.txt from the statistically-likely-usernames GitHub
![[../../assets/Making Target User List/image 4 170.png|image 4 170.png]]
- cpts said they used it to check over 48,000 usernames in just over 12 seconds
  
![[../../assets/Making Target User List/image 5 159.png|image 5 159.png]]
  
to get a user file (since -o didn’t want to work
- pipe kerbrute command into a file
- run the following to get user list from the output (looks like above)
```JavaScript
cat <userfile> | grep -oP '(?<=VALID USERNAME:\s)[^@]+' > users.txt
```
![[../../assets/Making Target User List/image 6 138.png|image 6 138.png]]
  
# Credentialed Enumeration to Build User List
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> --users
```