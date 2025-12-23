![image 98.png](assets/Sau/image%2098.png)
  
![image 1 65.png](assets/Sau/image%201%2065.png)
port 55555 has the webpage
  
![](assets/Sau/Screenshot_from_2025-02-21_13-52-24.png)
- can write html code with the double arrow icon
    
    - modifies the endpoint
    
![](assets/Sau/Screenshot_from_2025-02-21_13-53-13.png)
  
  
![image 2 64.png](assets/Sau/image%202%2064.png)
  
![image 3 57.png](assets/Sau/image%203%2057.png)
xss also works
  
![image 4 53.png](assets/Sau/image%204%2053.png)
request-baskets versions 1.2.1
  
this cve explains that we can exploit ssrf in order to gain access to internal services that we otherwise wouldn’t (i.e. port 80)
https://github.com/madhavmehndiratta/CVE-2023-27163
  
![image 5 52.png](assets/Sau/image%205%2052.png)
by using the CVE above, we can proxy connections to the basket and now see whats on port 80
  
by studying the maltrail (service port 80 is running) github, we can find files that we are able to now access through this proxy
[https://github.com/stamparm/maltrail/tree/master/html](https://github.com/stamparm/maltrail/tree/master/html)
![image 6 48.png](assets/Sau/image%206%2048.png)
- getting access to robots.txt
  
running the following exploit against the newly accessed maltrail service gives a reverse shell
https://github.com/spookier/Maltrail-v0.53-Exploit
![image 7 46.png](assets/Sau/image%207%2046.png)
![image 8 43.png](assets/Sau/image%208%2043.png)
  
![image 9 43.png](assets/Sau/image%209%2043.png)
maltrail.conf has user hashes?
  
![image 10 37.png](assets/Sau/image%2010%2037.png)
  
![image 11 35.png](assets/Sau/image%2011%2035.png)
  
in order to get an ssh session, create a .ssh folder with an authorized_keys file, and put the kali’s public key into that file, then just ssh with your private key
![image 12 28.png](assets/Sau/image%2012%2028.png)
  
![image 13 28.png](assets/Sau/image%2013%2028.png)
puma can run systemctl as sudo with no password
![image 14 26.png](assets/Sau/image%2014%2026.png)
- option (c) works here (i did it with !/bin/bash)
![image 15 24.png](assets/Sau/image%2015%2024.png)
![image 16 21.png](assets/Sau/image%2016%2021.png)