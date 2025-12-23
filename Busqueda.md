![image 121.png](assets/Busqueda/image%20121.png)
  
![image 1 87.png](assets/Busqueda/image%201%2087.png)
  
![image 2 86.png](assets/Busqueda/image%202%2086.png)
  
![image 3 79.png](assets/Busqueda/image%203%2079.png)
  
![image 4 75.png](assets/Busqueda/image%204%2075.png)
/search can’t use GET, but can use OPTIONS and POST
![image 5 74.png](assets/Busqueda/image%205%2074.png)
  
  
![image 6 70.png](assets/Busqueda/image%206%2070.png)
using searcher 2.4.0
  
https://github.com/nikn0laty/Exploit-for-Searchor-2.4.0-Arbitrary-CMD-Injection
  
![image 7 67.png](assets/Busqueda/image%207%2067.png)
  
well that was easy
  
![image 8 63.png](assets/Busqueda/image%208%2063.png)
  
![image 9 60.png](assets/Busqueda/image%209%2060.png)
  
![image 10 51.png](assets/Busqueda/image%2010%2051.png)
  
  
![image 11 48.png](assets/Busqueda/image%2011%2048.png)
shows gitea subdomain on /var/www/app/.git when doing git log
  
![image 12 41.png](assets/Busqueda/image%2012%2041.png)
version 1.18.0
  
![image 13 41.png](assets/Busqueda/image%2013%2041.png)
in the gitea config files /var/www/app/.git/config, it shows cody’s password
- now that we’re logged in
cody:jh1usoih2bkjaspwe92
  
![image 14 38.png](assets/Busqueda/image%2014%2038.png)
works for svc as well
- svc can run /usr/bin/python3 /opt/scripts/system-checkup.py * as root
  
![image 15 35.png](assets/Busqueda/image%2015%2035.png)
  
![image 16 31.png](assets/Busqueda/image%2016%2031.png)
  
![image 17 25.png](assets/Busqueda/image%2017%2025.png)
when doing docker-inspect, you can choose to see the environmental variables for the container
- mysql_root_password = jI86kGUuj87guWr3RyF
- mysql_password = yuiu1hoiu4i5ho1uh
  
mysql_password works for the administrator account on gitea
![image 18 23.png](assets/Busqueda/image%2018%2023.png)
  
and they have a scripts repo
![image 19 21.png](assets/Busqueda/image%2019%2021.png)
  
![image 20 18.png](assets/Busqueda/image%2020%2018.png)
checking the network settings for the db, we can see it runs on 172.19.0.3
  
![image 21 18.png](assets/Busqueda/image%2021%2018.png)
- nothing more interesting here
  
![image 22 15.png](assets/Busqueda/image%2022%2015.png)
in the full-checkup command, it uses the current working directory’s full-checkup.sh, which is why it failed
- we can use our own [full-checkup.sh](http://full-checkup.sh) to run it as root
  
![image 23 13.png](assets/Busqueda/image%2023%2013.png)