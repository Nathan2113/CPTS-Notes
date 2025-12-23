![image 133.png](assets/Mantis/image%20133.png)
![image 1 99.png](assets/Mantis/image%201%2099.png)
![image 2 98.png](assets/Mantis/image%202%2098.png)
  
![image 3 91.png](assets/Mantis/image%203%2091.png)
website on port 8080
  
![image 4 86.png](assets/Mantis/image%204%2086.png)
with a sign in page
  
![image 5 84.png](assets/Mantis/image%205%2084.png)
  
nothing on enum4linux
![image 6 80.png](assets/Mantis/image%206%2080.png)
  
hit on smb, but no anon login
- they’re using Windows server 2008 with SMBv1
![image 7 77.png](assets/Mantis/image%207%2077.png)
  
rpc access denied for anon
![image 8 73.png](assets/Mantis/image%208%2073.png)
  
ldap doesn’t allow anonymous binds
![image 9 70.png](assets/Mantis/image%209%2070.png)
  
that confirms it
![image 10 60.png](assets/Mantis/image%2010%2060.png)
  
they have an archive page on port 8080
![image 11 56.png](assets/Mantis/image%2011%2056.png)
- it’s powered by Orchard as well
  
couple orchard exploits
![image 12 47.png](assets/Mantis/image%2012%2047.png)
- two of which are asp based
  
![image 13 47.png](assets/Mantis/image%2013%2047.png)
![image 14 44.png](assets/Mantis/image%2014%2044.png)
  
![image 15 41.png](assets/Mantis/image%2015%2041.png)
input fields here
  
ok dude
![image 16 36.png](assets/Mantis/image%2016%2036.png)
  
open port on 1337
- this is why you scan all ports :)
  
IIS7
![image 17 30.png](assets/Mantis/image%2017%2030.png)
  
aspnet_client/system_web seems promising
![image 18 28.png](assets/Mantis/image%2018%2028.png)
  
  
  
![image 19 25.png](assets/Mantis/image%2019%2025.png)
  
![image 20 21.png](assets/Mantis/image%2020%2021.png)
  
![image 21 21.png](assets/Mantis/image%2021%2021.png)
ton of whitespace until you get this
  
admin:@dm!n_P@ssW0rd!
![image 22 18.png](assets/Mantis/image%2022%2018.png)
  
/admin finally got us somewhere
![image 23 15.png](assets/Mantis/image%2023%2015.png)
  
the creds for sa is part of the filename
![image 24 12.png](assets/Mantis/image%2024%2012.png)
  
this is very CTF
![image 25 8.png](assets/Mantis/image%2025%208.png)
  
![image 26 6.png](assets/Mantis/image%2026%206.png)
  
m$$ql_S@_P@ssW0rd!
![image 27 6.png](assets/Mantis/image%2027%206.png)
  
in the database
![image 28 6.png](assets/Mantis/image%2028%206.png)
  
  
set up and smb server and used dirtree to capture NTLMv2
![image 29 5.png](assets/Mantis/image%2029%205.png)
  
damn
![image 30 5.png](assets/Mantis/image%2030%205.png)
  
i guess you can select * from tables
![image 31 4.png](assets/Mantis/image%2031%204.png)
  
insane
![image 32 4.png](assets/Mantis/image%2032%204.png)
  
https://github.com/mubix/pykek
vulnerability allowing an unprivileged domain user to create a golden ticket
- need the user’s SID to execute
- can get SID with rpcclient
![image 33 3.png](assets/Mantis/image%2033%203.png)
  
can just use impacket apparently
- needs to use the FQDN of the server
- can get that from the nmap scan
![image 34 3.png](assets/Mantis/image%2034%203.png)
add FQDN to /etc/hosts
```JavaScript
10.10.10.52 mantis htb.local mantis.htb.local
```
  
![image 35 3.png](assets/Mantis/image%2035%203.png)