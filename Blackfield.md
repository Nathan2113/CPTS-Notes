![image 76.png](assets/Blackfield/image%2076.png)
Domain: BLACKFIELD.local0
- also try BLACKFIELD.local
  
nothing from rid-brute
![image 1 43.png](assets/Blackfield/image%201%2043.png)
  
- Ldap signing not enabled
![image 2 43.png](assets/Blackfield/image%202%2043.png)
  
signing into SMB anonymously shows a profiles$ folder that we have access to
- for some reason crackmapexec said we didn’t have access but we did?
- gives a list of users
![image 9.png](assets/Blackfield/image%209.png)
- nothing in any of their folders
  
Running this command will output just the users
- smbclient -N \\\\10.10.10.192\\profiles$ -c ‘ls’ | awk ‘{print $1}’ > users.txt
![image 1 3.png](assets/Blackfield/image%201%203.png)
  
Getting any pre-auth users
![image 3 37.png](assets/Blackfield/image%203%2037.png)
![image 4 33.png](assets/Blackfield/image%204%2033.png)
- ez money  
    
crack that 
![image 5 32.png](assets/Blackfield/image%205%2032.png)
![image 6 28.png](assets/Blackfield/image%206%2028.png)
User: support
Pass: #00^BlackKnight
  
support can access more shares
![image 7 27.png](assets/Blackfield/image%207%2027.png)
  
nothing in IPC$
![image 8 24.png](assets/Blackfield/image%208%2024.png)
NETLOGON also empty
![image 9 24.png](assets/Blackfield/image%209%2024.png)
SYSVOL has registry.pol
- gives password policy of domain
    
    - not useful now but could be for CPTC
    
  
enumdomusers gives 2 more users
![image 10 19.png](assets/Blackfield/image%2010%2019.png)
domain groups
![image 11 18.png](assets/Blackfield/image%2011%2018.png)
  
From the bloodhound output we can see it’s connecting to an LDAP server
- add the server into /etc/hosts
![image 12 13.png](assets/Blackfield/image%2012%2013.png)
  
- owned support account, and the support acount has first degree object control over another account
  
![image 13 13.png](assets/Blackfield/image%2013%2013.png)
![image 14 12.png](assets/Blackfield/image%2014%2012.png)
- can force change password on audit2020
[https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/acl-persistence-abuse](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/acl-persistence-abuse)
![image 15 11.png](assets/Blackfield/image%2015%2011.png)
![image 16 9.png](assets/Blackfield/image%2016%209.png)
- changed audit2020’s account password to “Password1”
  
![image 17 7.png](assets/Blackfield/image%2017%207.png)
- can access the forensics share
  
found domain admins in the forensics share
![image 18 7.png](assets/Blackfield/image%2018%207.png)
- Both are users
- not useful
  
in memory_analysis there’s an lsass.zip
- could extract passwords using pypykatz (kind of like using mimikatz)
  
pypykatz lsa minidump lsass.DMP
![image 19 6.png](assets/Blackfield/image%2019%206.png)
- ez money
![image 20 5.png](assets/Blackfield/image%2020%205.png)
- svc_backup is probably the next step
  
can use the NT part of the hash for svc_backup to log into SMB(weird)
![image 21 5.png](assets/Blackfield/image%2021%205.png)
- can log in with the “—pw-nt-hash” flag
  
- can also log in with evil-winrm
![image 22 5.png](assets/Blackfield/image%2022%205.png)
  
svc_backup has the following privs
![image 23 4.png](assets/Blackfield/image%2023%204.png)
  
- since they have BackupPrivilege, we can copy the sam and system
![image 24 3.png](assets/Blackfield/image%2024%203.png)
  
pass the hash with admin hash (LM part)
![image 25 3.png](assets/Blackfield/image%2025%203.png)
- the admin hash doesn’t work
  
dumping the NTDS.dit using this resource
[https://www.hackingarticles.in/windows-privilege-escalation-sebackupprivilege/](https://www.hackingarticles.in/windows-privilege-escalation-sebackupprivilege/)
- Refer to “**Exploiting Privilege on Domain Controller (Method 1)”**
    
    - can ctrl-f “unix2dos” to easily find it
    
  
pass the admin hash to login
- ez money