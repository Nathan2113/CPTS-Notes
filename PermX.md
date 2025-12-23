![image 71.png](assets/PermX/image%2071.png)
  
Subdomains
![image 1 38.png](assets/PermX/image%201%2038.png)
  
Directory Search
![image 2 38.png](assets/PermX/image%202%2038.png)
  
  
In the lms subdomain, you can see file uploads
![image 3 32.png](assets/PermX/image%203%2032.png)
  
There is a CVE on github for uploading files on Chamilo
![image 4 28.png](assets/PermX/image%204%2028.png)
  
running the scripts gets a reverse shell
![image 5 27.png](assets/PermX/image%205%2027.png)
  
creds on the chamilo config file
- /var/www/chamilo/app/config/configuration.php
![image 6 23.png](assets/PermX/image%206%2023.png)
User: chamilo
Pass: 03F6lY3uXAP2bkW8
  
use the password for user “mtz” to ssh into the server
![image 7 22.png](assets/PermX/image%207%2022.png)
  
can run [acl.sh](http://acl.sh) as root
![image 8 19.png](assets/PermX/image%208%2019.png)
  
[acl.sh](http://acl.sh) changed the permissions of a file, meaning we can change a root file to rwx for mtz
![image 9 19.png](assets/PermX/image%209%2019.png)
  
since we are able to change permissions, we can create a new user called root3 in the etc/passwd folder that has root privileges
![image 10 14.png](assets/PermX/image%2010%2014.png)
![image 11 14.png](assets/PermX/image%2011%2014.png)
can’t do execute for a permission on the txt file > that’s why it says permission denied