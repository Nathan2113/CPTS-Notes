![image 65.png](assets/Forest/image%2065.png)
  
crackmapexec ldap 10.10.10.161 -M ldap-signing
![image 1 33.png](assets/Forest/image%201%2033.png)
- Domain - htb.local
    
    - name - FOREST
    
- signing - NOT enforced
- SMBv1 - true
  
![image 2 33.png](assets/Forest/image%202%2033.png)
![image 3 28.png](assets/Forest/image%203%2028.png)
- ldapsearch -x -H ldap://<IP> -D ‘’ -w ‘’ -b “DC=<DOMAIN>,DC=<DOMAIN>” | grep user*
- for some reason this doesn’t give every user (should be 6 users)
trying enum4linux gave us the entire user list
![image 4 24.png](assets/Forest/image%204%2024.png)
- bottom 6 are the important ones
  
writing the users into a list, and using “GetNPUsers.py gives us a pre-authenticated user
![image 5 23.png](assets/Forest/image%205%2023.png)
  
cracking with hashcat gives us the password “s3rvice”
![image 6 19.png](assets/Forest/image%206%2019.png)
Username: svc-alfresco
Password: s3rvice
  
now we can see what smb shares the user can access with smbmap
![image 7 18.png](assets/Forest/image%207%2018.png)
- nothing in the shares
  
since port 5985 is open, we can get a shell using evil-winrm
![image 8 16.png](assets/Forest/image%208%2016.png)
  
winPEAS time :3
![image 9 16.png](assets/Forest/image%209%2016.png)
- gave us nothing
  
trying to use mimikatz on evil-winrm with the following commands
- /opt/privesc/powershell is a github repo
[https://github.com/clymb3r/PowerShell/tree/master](https://github.com/clymb3r/PowerShell/tree/master)
![image 10 12.png](assets/Forest/image%2010%2012.png)
- didn’t work
  
now trying bloodhound (specifically using SharpHound.exe)
![image 11 12.png](assets/Forest/image%2011%2012.png)
  
the bloodhound map shows
![image 12 9.png](assets/Forest/image%2012%209.png)
- exchange windows permissions allows the creation and modification of accounts
  
![image 13 9.png](assets/Forest/image%2013%209.png)
- john added to Exchange Windows Permissions group
  
now that john is part of the Exchange Windows Permissions group, we are able to give the user DcSync permissions, since the Exchange Windows Permissions group has WriteDacl permission on htb.local
![image 14 9.png](assets/Forest/image%2014%209.png)
![image 15 8.png](assets/Forest/image%2015%208.png)