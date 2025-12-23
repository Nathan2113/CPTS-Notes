![image 87.png](assets/LinkVortex/image%2087.png)
  
no extra directories
![image 1 54.png](assets/LinkVortex/image%201%2054.png)
  
but dev subdomain
![image 2 54.png](assets/LinkVortex/image%202%2054.png)
  
doesn’t really have anything yet
![image 3 47.png](assets/LinkVortex/image%203%2047.png)
fuzzing in dev subdomain shows .git is a redirect
![image 4 43.png](assets/LinkVortex/image%204%2043.png)
  
![image 5 42.png](assets/LinkVortex/image%205%2042.png)
:)
  
they’re using perl
![image 6 38.png](assets/LinkVortex/image%206%2038.png)
![image 7 37.png](assets/LinkVortex/image%207%2037.png)
  
using git-dumper gets us all the files on their site onto our local machine
![image 8 34.png](assets/LinkVortex/image%208%2034.png)
  
searching for “const password” in the dumped files gives us 2 passwords
![image 9 34.png](assets/LinkVortex/image%209%2034.png)
OctopiFociPilfer45
thisissupersafe
  
with some brute forcing we found an admin account at admin@linkvortex.htb
- now with this PoC we can read files in the system
https://github.com/0xDTC/Ghost-5.58-Arbitrary-File-Read-CVE-2023-40028
  
![image 10 29.png](assets/LinkVortex/image%2010%2029.png)
  
checking the dockerfile.ghost shows us a configuration file path
![image 11 28.png](assets/LinkVortex/image%2011%2028.png)
putting that directory into the PoC shows us some user information
  
something on port 2368
![image 12 22.png](assets/LinkVortex/image%2012%2022.png)
and user credentials
![image 13 22.png](assets/LinkVortex/image%2013%2022.png)
bob:fibber-talented-worth
  
![image 14 20.png](assets/LinkVortex/image%2014%2020.png)
- ez dubbins
  
![image 15 18.png](assets/LinkVortex/image%2015%2018.png)
sudo -l shows us we can run /opt/ghost/clean_symlink.sh as root and *png as root
![image 16 16.png](assets/LinkVortex/image%2016%2016.png)
![image 17 14.png](assets/LinkVortex/image%2017%2014.png)
  
chatgpt shows some good ways to exploit this
![image 18 13.png](assets/LinkVortex/image%2018%2013.png)
  
trying to recursively link files so that it reads the content of the sensitive file
![image 19 12.png](assets/LinkVortex/image%2019%2012.png)
- getting close i think
  
  
doing these commands in order will return the root.txt contents
- ln -s /tmp/../../root/root.txt /home/bob/symlink1.png
- ln -s /home/bob/symlink1.png /home/bob/symlink2.png
- sudo CHECK_CONTENT=true /usr/bin/bash /opt/ghost/clean_symlink.sh symlink2.png