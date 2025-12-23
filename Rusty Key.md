![image 103.png](assets/Rusty%20Key/image%20103.png)
  
works with nxc but not cme
![image 1 69.png](assets/Rusty%20Key/image%201%2069.png)
![image 2 68.png](assets/Rusty%20Key/image%202%2068.png)
  
![image 3 61.png](assets/Rusty%20Key/image%203%2061.png)
- dc name is dc.rustykey.htb, not dc01
  
![image 4 57.png](assets/Rusty%20Key/image%204%2057.png)
- Domain SID S-1-5-21-3316070415-896458127-4139322052
- crackmapexec ldap <IP> -u <user> -p <pass> -k
  
  
![image 5 56.png](assets/Rusty%20Key/image%205%2056.png)
- timeroast :)
https://github.com/SecuraBV/Timeroast
  
and crack it with the [timecrack.py](http://timecrack.py) file in the github
![image 6 52.png](assets/Rusty%20Key/image%206%2052.png)
  
can also use hashcat mode 31300
  
RID 1125 corresponds to IT-Computer3
![image 7 50.png](assets/Rusty%20Key/image%207%2050.png)
  
which has addself to helpdesk
![image 8 47.png](assets/Rusty%20Key/image%208%2047.png)
  
  
nxc alternative
- netexec smb <IP> -d <DOMAIN> -u <user> -p <pass> -M timeroast