![image 83.png](assets/Administrator/image%2083.png)
  
nothing in smb or enum4linux
![image 1 50.png](assets/Administrator/image%201%2050.png)
  
Domain: administrator.htb
  
![image 2 50.png](assets/Administrator/image%202%2050.png)
assumed breach smh my head
  
![image 3 44.png](assets/Administrator/image%203%2044.png)
  
users from CME
![image 4 40.png](assets/Administrator/image%204%2040.png)
  
![image 5 39.png](assets/Administrator/image%205%2039.png)
no pre-auth and no kerberoasting
  
force change password
![image 6 35.png](assets/Administrator/image%206%2035.png)
  
get olivia’s tgt
![](assets/Administrator/Screenshot_from_2024-11-09_14-07-24.png)
![](assets/Administrator/Screenshot_from_2024-11-09_14-07-43.png)
![image 7 34.png](assets/Administrator/image%207%2034.png)
![](assets/Administrator/Screenshot_from_2024-11-09_14-11-11.png)
ez money
  
![image 8 31.png](assets/Administrator/image%208%2031.png)
force change password for benjamin
  
![image 9 31.png](assets/Administrator/image%209%2031.png)
  
michael can psremote
![image 10 26.png](assets/Administrator/image%2010%2026.png)
  
benjamin is part of the share moderators
![image 11 25.png](assets/Administrator/image%2011%2025.png)
- if we can make the shares writeable for olivia, we can probably get the user flag
- that didnt work
  
ftp server with benjamin
![image 12 19.png](assets/Administrator/image%2012%2019.png)
![image 13 19.png](assets/Administrator/image%2013%2019.png)
![image 14 18.png](assets/Administrator/image%2014%2018.png)
tekieromucho
  
open passsword manager with pwsafe package
![image 15 16.png](assets/Administrator/image%2015%2016.png)
  
emily has GenericWrite over ethan
- do a targeted kerberoast
  
python3 [targetedKerberoast.py](http://targetedkerberoast.py/) -v -d 'administrator.htb' -u 'emily' -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb'
![image 16 14.png](assets/Administrator/image%2016%2014.png)
  
crack that 
![image 17 12.png](assets/Administrator/image%2017%2012.png)
  
ethan has DCSync
- secretsdump
![image 18 11.png](assets/Administrator/image%2018%2011.png)
  
![image 19 10.png](assets/Administrator/image%2019%2010.png)
ez box
  
evil-winrm -i 10.10.11.42 -u Administrator -H ‘3dc553c34b9fd20bd016e098d2d2fd2e’