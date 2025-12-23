![image 74.png](assets/Sea/image%2074.png)
  
Apache 2.4.41
![image 1 41.png](assets/Sea/image%201%2041.png)
  
Contact page has user fields
![image 2 41.png](assets/Sea/image%202%2041.png)
  
Directoy Fuzz
![image 3 35.png](assets/Sea/image%203%2035.png)
  
fuzzing in /data reveals a /files
![image 4 31.png](assets/Sea/image%204%2031.png)
  
  
if you fuzz for /themes/bike you’ll see a readme
![image 5 30.png](assets/Sea/image%205%2030.png)
website is using the WonderCMS
- checking /themes/bike/version reveals it’s WonderCMS version 3.2.0
![image 6 26.png](assets/Sea/image%206%2026.png)
  
CVE for XSS to RCE
- works because an “admin” is clicking our XSS link we send that the CVE crafts
https://github.com/thefizzyfish/CVE-2023-41425-wonderCMS_RCE
![image 7 25.png](assets/Sea/image%207%2025.png)
  
database.js in /var/www/sea/data contains a password
![image 8 22.png](assets/Sea/image%208%2022.png)
- bcrypt - refer to bcrypt notes
- since other places in the file replace “/” with “\/”, we need to modify the hash in order to be correct
    
    - meaning change “\/” back to just “/”
    
  
![image 9 22.png](assets/Sea/image%209%2022.png)
![image 10 17.png](assets/Sea/image%2010%2017.png)
pass: mychemicalromance
- try this with the users found on the system
![image 11 16.png](assets/Sea/image%2011%2016.png)
  
worked with amay
![image 12 12.png](assets/Sea/image%2012%2012.png)
  
linpeas shows outdated sudo version
![image 13 12.png](assets/Sea/image%2013%2012.png)
![image 14 11.png](assets/Sea/image%2014%2011.png)
![image 15 10.png](assets/Sea/image%2015%2010.png)
  
doing netstat -a will show all the ports onthe server
![image 16 8.png](assets/Sea/image%2016%208.png)
http-alt (8080) shows up a lot
- that means there’s another port running that we otherwise don’t have access to
  
ssh -L <YOUR_PORT>:localhost:<THEIR_PORT> <user>@ip
- forwards their port to the specified host
- pivoting
![image 17 6.png](assets/Sea/image%2017%206.png)
  
going to the port and logging in with amay’s creds gives us a log monitor
![image 18 6.png](assets/Sea/image%2018%206.png)
- using burp to intercept the request can get us the root flag
![image 19 5.png](assets/Sea/image%2019%205.png)
- needs suspicious activity to trip the sensor
    
    - can just use a semicolon along with a command