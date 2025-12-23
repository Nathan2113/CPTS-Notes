![image 113.png](assets/Editor/image%20113.png)
  
![image 1 79.png](assets/Editor/image%201%2079.png)
wiki’s robots.txt
  
![image 2 78.png](assets/Editor/image%202%2078.png)
admin page found from robots.txt
  
found this CVE from the version number at bottom of login page
https://github.com/gunzf0x/CVE-2025-24893
  
![image 3 71.png](assets/Editor/image%203%2071.png)
  
![image 4 67.png](assets/Editor/image%204%2067.png)
first try?
  
  
/etc/xwiki/hibernate.xfg.xml has user creds
![image 5 66.png](assets/Editor/image%205%2066.png)
theEd1t0rTeam99
  
  
![image 6 62.png](assets/Editor/image%206%2062.png)
reuse creds
  
  
![image 7 60.png](assets/Editor/image%207%2060.png)
  
![image 8 56.png](assets/Editor/image%208%2056.png)
can log into sql using creds found earlier
  
  
![image 9 53.png](assets/Editor/image%209%2053.png)
edit.conf → editor hahaha
  
![image 10 45.png](assets/Editor/image%2010%2045.png)
ndsudo has the SUID bit set
  
follow this CVE github
https://github.com/AliElKhatteb/CVE-2024-32019-POC
  
make the poc, change to your ip, compile, then send it over to victim machine
![image 11 42.png](assets/Editor/image%2011%2042.png)
  
once its on the victim, change the path variable and run the exploit after setting up a listener
  
![image 12 35.png](assets/Editor/image%2012%2035.png)
  
![image 13 35.png](assets/Editor/image%2013%2035.png)