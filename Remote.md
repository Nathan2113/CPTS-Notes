![image 118.png](assets/Remote/image%20118.png)
  
  
port 2049 is network file shares, which we are able to mount to our system
![image 1 84.png](assets/Remote/image%201%2084.png)
  
![image 2 83.png](assets/Remote/image%202%2083.png)
  
doing some research into umbraco (the cms that the webserver is using) i found the database name
![image 3 76.png](assets/Remote/image%203%2076.png)
Umbraco.sdf
  
![image 4 72.png](assets/Remote/image%204%2072.png)
found it here using vscode
  
api key: AIzaSyBSjIm2tkaskXtivVDbvlXcWkP6JDCoqA4
  
under membership provider in web config we can see they are using legacy hashing algorithms (HMAC-SHA1)
![image 5 71.png](assets/Remote/image%205%2071.png)
![image 6 67.png](assets/Remote/image%206%2067.png)
  
at the very top of the umbraco.sdf file there is a sha-1 hash for the admin account
![image 7 64.png](assets/Remote/image%207%2064.png)
![image 8 60.png](assets/Remote/image%208%2060.png)
  
![image 9 57.png](assets/Remote/image%209%2057.png)
![image 10 48.png](assets/Remote/image%2010%2048.png)
  
![image 11 45.png](assets/Remote/image%2011%2045.png)
using umbraco version 7.12.4
  
https://github.com/0xDTC/Umbraco-CMS-7.12.4-Authenticated-RCE
rce for 7.12.4 (requires authentication)
  
have to run this exploit with the IP, using the hostname didn’t work
![image 12 38.png](assets/Remote/image%2012%2038.png)
  
[https://www.bordergate.co.uk/windows-privilege-escalation/](https://www.bordergate.co.uk/windows-privilege-escalation/)
![image 13 38.png](assets/Remote/image%2013%2038.png)
  
can exploit GodPotato when you have the SeImpersonatePrivilege
- juicypotato didn’t work, so i tried godpotato instead
  
https://github.com/BeichenDream/GodPotato/releases/
- the link still works
  
when running the command, it has trouble parsing the command because of how the RCE works, so i had to make a bat file that will run the command
- this happened because i was trying to use single and double quotes together, and the PoC didn’t like that
  
![image 14 35.png](assets/Remote/image%2014%2035.png)
  
now upload the files
  
![image 15 32.png](assets/Remote/image%2015%2032.png)
  
set up a netcat listener, then run the exploit
![image 16 28.png](assets/Remote/image%2016%2028.png)
![image 17 23.png](assets/Remote/image%2017%2023.png)