![image 89.png](assets/Bizness/image%2089.png)
  
![image 1 56.png](assets/Bizness/image%201%2056.png)
- going to port 80 redirects to 443
  
![image 2 56.png](assets/Bizness/image%202%2056.png)
- no directories
  
![image 3 49.png](assets/Bizness/image%203%2049.png)
  
service name on bottom
![image 4 45.png](assets/Bizness/image%204%2045.png)
  
research for ofbiz shows authbypass
https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass
  
![image 5 44.png](assets/Bizness/image%205%2044.png)
- detection script shows that it’s vulnerable
  
![image 6 40.png](assets/Bizness/image%206%2040.png)
- pinging myself and checking tcpdump shows that i’m getting a response back from RCE
  
![image 7 39.png](assets/Bizness/image%207%2039.png)
- nc with /bin/bash gets a shell
  
![image 8 36.png](assets/Bizness/image%208%2036.png)
- i changed gradlew to nc to my computer, trying to find a way for ofbiz.service to execute the file
  
![image 9 36.png](assets/Bizness/image%209%2036.png)
- default creds for ofbiz database
  
  
![image 10 31.png](assets/Bizness/image%2010%2031.png)
- cat /opt/ofbiz/runtime/data/derby/ofbiz/seg0/c54d0.dat
  
https://github.com/duck-sec/Apache-OFBiz-SHA1-Cracker
![image 11 30.png](assets/Bizness/image%2011%2030.png)
  
su root :3