![image 66.png](assets/Sauna/image%2066.png)
  
Domain: EGOTISTICAL-BANK.LOCAL0
  
No anon SMB or RPC
  
No interesting directories or subdomains
![image 1 34.png](assets/Sauna/image%201%2034.png)
  
going to the about us page shows us employee names
- use username-anarchy to create a user list
https://github.com/urbanadventurer/username-anarchy
  
using the created wordlist to get pre-auth users gives us a hash for fsmith
![image 2 34.png](assets/Sauna/image%202%2034.png)
- [GetNPUsers.py](http://getnpusers.py/) EGOTISTICAL-BANK.LOCAL/ -usersfile users.txt -dc-ip 10.10.10.175 -no-pass
  
crack the hash
![image 3 29.png](assets/Sauna/image%203%2029.png)
![image 4 25.png](assets/Sauna/image%204%2025.png)
User: fsmith  
Pass: Thestrokes23
  
evil-winrm with new creds
![image 5 24.png](assets/Sauna/image%205%2024.png)
  
fsmith privs
![image 6 20.png](assets/Sauna/image%206%2020.png)
  
fsmith can access some shares (read only)
![image 7 19.png](assets/Sauna/image%207%2019.png)
  
enumdomusers
![image 8 17.png](assets/Sauna/image%208%2017.png)
  
winPEAS gives computer name - SAUNA
![image 9 17.png](assets/Sauna/image%209%2017.png)
  
winPEAS shows an autologon user with creds
![image 10 13.png](assets/Sauna/image%2010%2013.png)
User: svc_loanmgr
Pass: Moneymakestheworldgoround!
  
  
svc_loanmanager has DS-Replication-Get-Changes and DS-Replication-Get-Changes-All
![image 11 13.png](assets/Sauna/image%2011%2013.png)
  
![image 12 10.png](assets/Sauna/image%2012%2010.png)
  
  
PassTheHash
![image 13 10.png](assets/Sauna/image%2013%2010.png)
[[Authority]]