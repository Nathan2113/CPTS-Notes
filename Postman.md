![image 115.png](assets/Postman/image%20115.png)
  
![image 1 81.png](assets/Postman/image%201%2081.png)
upload directory
  
![image 2 80.png](assets/Postman/image%202%2080.png)
  
  
![image 3 73.png](assets/Postman/image%203%2073.png)
  
![image 4 69.png](assets/Postman/image%204%2069.png)
port 10000 is hosting a website using https
- webmin login page
  
![image 5 68.png](assets/Postman/image%205%2068.png)
can connect to redis on port 6379
  
[https://hackviser.com/tactics/pentesting/services/redis](https://hackviser.com/tactics/pentesting/services/redis)
going to exploit redis to upload our public ssh key and ssh into the machine directly
![image 6 64.png](assets/Postman/image%206%2064.png)
  
![image 7 62.png](assets/Postman/image%207%2062.png)
  
![image 8 58.png](assets/Postman/image%208%2058.png)
  
/opt has a private key in there, but we need the passphrase
![image 9 55.png](assets/Postman/image%209%2055.png)
  
![image 10 47.png](assets/Postman/image%2010%2047.png)
do have access to some webmin directories
  
![image 11 44.png](assets/Postman/image%2011%2044.png)
can access /usr/share/webmin/webmin, but not /etc/webmin/webmin
- maybe a copy that we have access to?
  
![image 12 37.png](assets/Postman/image%2012%2037.png)
ssh2john on the private key we found earlier
  
![image 13 37.png](assets/Postman/image%2013%2037.png)
computer2008 is the password for the ssh key
  
![image 14 34.png](assets/Postman/image%2014%2034.png)
can switch user to Matt and use the same password, if you try to ssh with the key it won’t let you in
  
![image 15 31.png](assets/Postman/image%2015%2031.png)
can now log into webmin with Matt as well
  
follow this exploit to get root
https://github.com/bkaraceylan/CVE-2019-12840_POC