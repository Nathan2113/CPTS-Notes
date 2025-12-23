Scan:
- 53 - domain
- 88 kerberos
- 135 - rpc
- 139 net-bios
- 389 - ldap
- 445 - smb
- 464 ncacn_http?
- 593 - rpc
- 636 - ssl/ldap
- 3268 - ldap
- 3269 - ldap
- 5985 - winrm
- 60812 - rpc
  
Enumerating SMB shares anonymously
- found a letter from HR
![image 72.png](assets/Cicada/image%2072.png)
![image 1 39.png](assets/Cicada/image%201%2039.png)
- Default password: Cicada$M6Corpb*@Lp\#nZp!8
  
Enumerating RID’s through nxc (smb) - crackmapexec can do the same thing
![image 2 39.png](assets/Cicada/image%202%2039.png)
- “nxc smb 10.10.11.35 -u ‘meow’ -p ‘’ —rid-brute
  
michael.wrightson can read NETLOGON and SYSVOL
![image 3 33.png](assets/Cicada/image%203%2033.png)
  
crackmapexec smb 10.10.11.35 -u ‘michael.wrightson’ -p ‘Cicada$M6Corpb*@Lp\#nZp!8’ —users
- shows user information → david.orelious has his password in the description
  
david is able to read the Dev share, which has a backup script with credentials for emily
![image 4 29.png](assets/Cicada/image%204%2029.png)
  
emily can log in through winrm
![image 5 28.png](assets/Cicada/image%205%2028.png)
  
and she has more privs than she should
![image 6 24.png](assets/Cicada/image%206%2024.png)
  
exploiting SeBackupPrivilege steps shown here:
https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/SeBackupPrivilege.md
  
creating new temp directory (so that we can write) and copy the SAM and SYSTEM
![image 7 23.png](assets/Cicada/image%207%2023.png)
  
download SAM and SYSTEM to kali
![image 8 20.png](assets/Cicada/image%208%2020.png)
  
dump them 
![image 9 20.png](assets/Cicada/image%209%2020.png)
- impacket-secretsdump -sam sam.hive -system system.hive LOCAL
  
Login with PassTheHash
- use the LM part of the hash
![image 10 15.png](assets/Cicada/image%2010%2015.png)
  
ez money