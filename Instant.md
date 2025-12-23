![image 92.png](assets/Instant/image%2092.png)
  
download the instant.apk from the website and unzip it with apktool
- apktool d instant.apk
  
nothing of note for directories or subdomains
![image 1 59.png](assets/Instant/image%201%2059.png)
![](assets/Instant/Screenshot_from_2025-02-03_13-38-13.png)
  
dirty harry :)
![](assets/Instant/Screenshot_from_2025-02-03_14-50-08.png)
![image 2 59.png](assets/Instant/image%202%2059.png)
  
![image 3 52.png](assets/Instant/image%203%2052.png)
- api key :3
    
    - wasn’t the API key :(
    
  
- search “authentication” to find the API key
![image 4 48.png](assets/Instant/image%204%2048.png)
- eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwicm9sZSI6IkFkbWluIiwid2FsSWQiOiJmMGVjYTZlNS03ODNhLTQ3MWQtOWQ4Zi0wMTYyY2JjOTAwZGIiLCJleHAiOjMzMjU5MzAzNjU2fQ.v0qyyAqDSgyoNFHU7MgRQcDA0Bw99_8AEXKGtWZ6rYA
  
now use the api functionality to dump users
![image 5 47.png](assets/Instant/image%205%2047.png)
  
![image 6 43.png](assets/Instant/image%206%2043.png)
- adding our own user with the role “instantAdmin”
- Admin:87348
    
    - wallet id - f0eca6e5-783a-471d-9d8f-0162cbc900db
    
- shirohige:42845
    
    - wallet id - 458715c9-b15e-467b-8a3d-97bc3fcf3c11
    
  
- email: hacked@htb.com
- pass: letmein
- pin: 54321
- role: instantAdmin
- username: hacked
- access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6NSwicm9sZSI6Imluc3RhbnRBZG1pbiIsIndhbElkIjoiZGVhOGY0ZGQtOWQ2MC00Zjc4LTkxMWUtZTY2ZGNjYWMzMTdkIiwiZXhwIjoxNzM4NjY2NDY5fQ.-ntcHFPvjrAKSO8w2Jbn66A-X-Wdne3KE_XCYl_n-xM
  
listing the logs
![image 7 42.png](assets/Instant/image%207%2042.png)
- there is a log from shirohige called “1.log”
![image 8 39.png](assets/Instant/image%208%2039.png)
- reading the log
  
can bin walk with the logs api
![image 9 39.png](assets/Instant/image%209%2039.png)
  
![image 10 34.png](assets/Instant/image%2010%2034.png)
- user flag :3
  
![image 11 32.png](assets/Instant/image%2011%2032.png)
- get shirohige’s ssh private key
    
    - /home/shirohige/.ssh/id_rsa
    
  
![](assets/Instant/Screenshot_from_2025-02-03_18-53-01.png)
- if you get this error, change the permissions of the key
- but anyways i’m in :)
  
![](assets/Instant/Screenshot_from_2025-02-03_18-57-40.png)
- was able to get into their virtual environment
  
![image 12 25.png](assets/Instant/image%2012%2025.png)
![image 13 25.png](assets/Instant/image%2013%2025.png)
- VeryStrongS3cretKeyY0uC4NTGET
  
admin hash from linpeas
![image 14 23.png](assets/Instant/image%2014%2023.png)
pbkdf2:sha256:600000$I5bFyb0ZzD69pNX8$e9e4ea5c280e0766612295ab9bff32e5fa1de8f6cbb6586fab7ab7bc762bd978
  
/opt/backups/Solar-PuTTY/sessions-backup.dat
- want to decrypt this file with the key
    
    - VeryStrongS3cretKeyY0uC4NTGET
    
  
![image 15 21.png](assets/Instant/image%2015%2021.png)
- didn’t even need the key :)
  
root:12**24nzC!r0c%q12
  
takeaways:
- when looking at backup files from linpeas make note of the users that can access what
![image 16 19.png](assets/Instant/image%2016%2019.png)
- this is a dead giveaway