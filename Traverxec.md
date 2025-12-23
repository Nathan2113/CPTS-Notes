![image 105.png](assets/Traverxec/image%20105.png)
  
![image 1 71.png](assets/Traverxec/image%201%2071.png)
  
  
![image 2 70.png](assets/Traverxec/image%202%2070.png)
404 reveals nostromo 1.9.6
  
ffuf is throttled so i’m not able to get subdomains or directories
  
some research into nostromo 1.9.6 reveals CVE-2019-16278
https://github.com/aN0mad/CVE-2019-16278-Nostromo_1.9.6-RCE
  
![image 3 63.png](assets/Traverxec/image%203%2063.png)
easy money
  
![image 4 59.png](assets/Traverxec/image%204%2059.png)
  
![image 5 58.png](assets/Traverxec/image%205%2058.png)
easy money
  
![image 6 54.png](assets/Traverxec/image%206%2054.png)
this box may be too easy
  
  
![image 7 52.png](assets/Traverxec/image%207%2052.png)
crypt hashes take forever man
  
![image 8 49.png](assets/Traverxec/image%208%2049.png)
david:Nowonly4me
  
i cracked his password but am not sure where to use it
- doesn’t work for ssh or switching users
  
![image 9 46.png](assets/Traverxec/image%209%2046.png)
in the homedirs, there is public_www, meaning that a URL like “http://traverxec.htb/~david/” will map to “/home/david/public_www/”
  
![image 10 40.png](assets/Traverxec/image%2010%2040.png)
abuse mentioned above in action
  
![image 11 38.png](assets/Traverxec/image%2011%2038.png)
  
![image 12 31.png](assets/Traverxec/image%2012%2031.png)
since we have access to this file in the browser, then we should have access to the folder where it’s stored (in david’s directory)
  
  
![image 13 31.png](assets/Traverxec/image%2013%2031.png)
—strip-components=2 gets rid of the first 2 directories (/home/david), meaning that the extracted archive will save directory into /tmp because i am using -C
  
![image 14 29.png](assets/Traverxec/image%2014%2029.png)
  
![image 15 27.png](assets/Traverxec/image%2015%2027.png)
using ssh2john we can get the hash for the passphrase
  
![image 16 24.png](assets/Traverxec/image%2016%2024.png)
  
![image 17 20.png](assets/Traverxec/image%2017%2020.png)
hunter worked :3
  
![image 18 19.png](assets/Traverxec/image%2018%2019.png)
  
![image 19 18.png](assets/Traverxec/image%2019%2018.png)
the server_stats.sh file in /home/david/bin calls cat without the absolute path, meaning if we make an executable file called cat in the bin directory and root runs the server_stats.sh script then we can get a root shell
- this wasn’t the way :(
  
instead we can just do this command that is in the server-stats.sh
- for some reason you need the WHOLE thing, doing /usr/bin/sudo /usr/bin/journalctl by itself doesn’t work
![image 20 15.png](assets/Traverxec/image%2020%2015.png)
once we’re in journalctl, we can type “!/bin/bash” and it’ll pop a root shell since david can run this as root without a password
![image 21 15.png](assets/Traverxec/image%2021%2015.png)