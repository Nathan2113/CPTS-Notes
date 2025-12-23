![image 106.png](assets/Blunder/image%20106.png)
ftp closed, gonna probably go in once i have a session
  
![image 1 72.png](assets/Blunder/image%201%2072.png)
apache 2.4.41
  
![image 2 71.png](assets/Blunder/image%202%2071.png)
  
  
![image 3 64.png](assets/Blunder/image%203%2064.png)
  
![image 4 60.png](assets/Blunder/image%204%2060.png)
site using bludit
  
  
![image 5 59.png](assets/Blunder/image%205%2059.png)
github
  
![image 6 55.png](assets/Blunder/image%206%2055.png)
![image 7 53.png](assets/Blunder/image%207%2053.png)
dirsearch revealed todo.txt, which shows the user fergus
  
  
there’s a CVE associated with bludit that involves brute forcing the login page
https://github.com/ColdFusionX/CVE-2019-17240_Bludit-BF-Bypass
  
we have the username fergus, but we need to get the password somehow
- going to use cewl on the website to scrape for potential passwords
![image 8 50.png](assets/Blunder/image%208%2050.png)
![image 9 47.png](assets/Blunder/image%209%2047.png)
![image 10 41.png](assets/Blunder/image%2010%2041.png)
  
fergus:RolandDeschain
![image 11 39.png](assets/Blunder/image%2011%2039.png)
  
![image 12 32.png](assets/Blunder/image%2012%2032.png)
metasploit module for file upload shell
  
![image 13 32.png](assets/Blunder/image%2013%2032.png)
  
![image 14 30.png](assets/Blunder/image%2014%2030.png)
![image 15 28.png](assets/Blunder/image%2015%2028.png)
  
three hashes (hugo is the most promising)
- admin:bfcc887f62e36ea019e3295aafb8a3885966e265
- author:be5e169cdf51bd4c878ae89a0a89de9cc0c9d8c7
- hugo:faca404fd5c0a31cf1897b823c695c85cffeb98d
  
![image 16 25.png](assets/Blunder/image%2016%2025.png)
hugo:Password120
  
![image 17 21.png](assets/Blunder/image%2017%2021.png)
  
![image 18 20.png](assets/Blunder/image%2018%2020.png)
note in ftp (ftp is in the root folder)
  
![image 19 19.png](assets/Blunder/image%2019%2019.png)
config.json
  
in the config folder there was a config gzip that had a buzz.wav
  
  
![image 20 16.png](assets/Blunder/image%2020%2016.png)
- hugo is able to run /bin/bash with sudo to turn into any user except root (as seen here)
    
    - this is indicated by the “!root”
    
  
  
![image 21 16.png](assets/Blunder/image%2021%2016.png)
shaun is part of the lxd group
- linux container manager
  
can abuse this by creating a container on the system, then breaking out of that container to get root
https://github.com/saghul/lxd-alpine-builder
- cd into the folder after cloning and do “./build-alpine”
  
all of this is a red herring and this box kinda sucks but here’s the way to do it :)
- the box is running Sudo version 1.8.25p1
- vulnerable to CVE-2019-14287
https://github.com/shallvhack/Sudo-Security-Bypass-CVE-2019-14287
![image 22 13.png](assets/Blunder/image%2022%2013.png)