![image 111.png](assets/Soccer/image%20111.png)
  
![image 1 77.png](assets/Soccer/image%201%2077.png)
- only directory is tiny
  
![image 2 76.png](assets/Soccer/image%202%2076.png)
- no subdomains
  
![image 3 69.png](assets/Soccer/image%203%2069.png)
- tiny leads to login page
  
![image 4 65.png](assets/Soccer/image%204%2065.png)
- on the login page, the default creds work
- admin:admin@123
  
![image 5 64.png](assets/Soccer/image%205%2064.png)
- navigate to /tiny/uploads and upload a php reverse shell
- go to /shell.php after setting a listener to get a reverse shell
  
![image 6 60.png](assets/Soccer/image%206%2060.png)
  
![image 7 58.png](assets/Soccer/image%207%2058.png)
  
![image 8 54.png](assets/Soccer/image%208%2054.png)
- privesc for once i get the player user
  
there is a websocket server running on port 9091, you can find the subdomain by searching the nginx config files
- subdomain is soc-player.soccer.htb
  
![image 9 51.png](assets/Soccer/image%209%2051.png)
  
going to the subdomain allows you to initiate a websocket connection to another login page
![image 10 44.png](assets/Soccer/image%2010%2044.png)
create an account and login to see a ticket system being used
  
![image 11 41.png](assets/Soccer/image%2011%2041.png)
enter your ticket number with intercept on
  
intercepting with burp shows you the parameters passing through the websockets
![image 12 34.png](assets/Soccer/image%2012%2034.png)
  
use this information with sqlmap to start fuzzing for SQLi
![image 13 34.png](assets/Soccer/image%2013%2034.png)
  
  
  
![image 14 32.png](assets/Soccer/image%2014%2032.png)
from before we know that player can run /usr/bin/doas as root, but we need to find a dstat location that they can write in (in this case it’s /usr/loca/share/dstat)
  
using the dstat information from before, we can do the sudo section on GTFObins
[https://gtfobins.github.io/gtfobins/dstat/](https://gtfobins.github.io/gtfobins/dstat/)
![image 15 29.png](assets/Soccer/image%2015%2029.png)
  
![image 16 26.png](assets/Soccer/image%2016%2026.png)
we have to use doas, not sudo to complete the commands