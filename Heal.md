![image 88.png](assets/Heal/image%2088.png)
  
![image 1 55.png](assets/Heal/image%201%2055.png)
api subdomain
  
no new directories in api subdomain
![image 2 55.png](assets/Heal/image%202%2055.png)
  
![image 3 48.png](assets/Heal/image%203%2048.png)
nginx 1.18.0
  
  
api.heal.htb shows version numbers and what api they’re using
![image 4 44.png](assets/Heal/image%204%2044.png)
![image 5 43.png](assets/Heal/image%205%2043.png)
  
they also have a take-survey subdomain
- on the resume builder, click “survey” and follow the links
![image 6 39.png](assets/Heal/image%206%2039.png)
![image 7 38.png](assets/Heal/image%207%2038.png)
  
leads here
![image 8 35.png](assets/Heal/image%208%2035.png)
  
![image 9 35.png](assets/Heal/image%209%2035.png)
- themes led to nothing
  
  
when finishing a survey, refresh the page and you get an admin email
![image 10 30.png](assets/Heal/image%2010%2030.png)
ralph@heal.htb
  
![image 11 29.png](assets/Heal/image%2011%2029.png)
download :)
  
trying to create a pdf and changing the filepath in burp gets you some information
![image 12 23.png](assets/Heal/image%2012%2023.png)
- i pulled their database.yml by changing the request to
    
    - GET /download?filename=../../config/database.yml HTTP/1.1
    
  
/config/credentials.yml.enc has a base64 string
![image 13 23.png](assets/Heal/image%2013%2023.png)
```Python
1dMkoxx+3u+vK2g1BWntnRZqGj16vLi5rQJlP/P+pcIpGeK7b12TC2UWjrdx+PoC3iYnMWS2QLK5jnBnaNXDHpEL9oDgc6Ul/9/ghl+3g4AzaFeHy1/yG6SMxA11CMmQhTcSGj1jBMyCT7dgmV6/hfCyb933QHukceAV1NVHqLH9Tcd+WnB3okQhD3NUOLhZ3ivc3wr2pyvxX7ym5kLIjSuHNwRcmMwcXS3e26Bc3Lk9ghUq795a90WfGtV7cIa2TzdY5lbMHHi167IP3zzpUvmY0AcR+WmXHt35WjktrELPe7hR83MRHwTrWt3OmqafsPBufCl1oUY1K2sEIJ8VQjHhrP870ASSS3BpEiGSdrCU53jNVIquJwaE8lg0p3phhMbLXYVVT9QZO1banDh5avfcmSEM--dsbT6QUqCzyNMigC--9SEHtK8HjvpnlvfmWIqoMg==
```
```Python
23d5052b447ee9376809464f8c141bdf
```
  
  
using these with this github gives us a decrypted string
https://github.com/mbadanoiu/Credentials.yml.enc_Decryptor
![image 14 21.png](assets/Heal/image%2014%2021.png)
```Python
7c54b3ab1c9f9d5037c8f8c856b4be85f4eca365430232a6743965665fbeec9c30e86dad314aa54ed90986223bc7841c89c2442a0e7e6b546a9366f4d3d8dc2d
```
![image 15 19.png](assets/Heal/image%2015%2019.png)
- potential hash types
  
![image 16 17.png](assets/Heal/image%2016%2017.png)
found database on /storage/development.sqlite3
  
ralph@heal.htb$2a$12$dUZ/O7KJT3.zE4TOK8p4RuxH3t.Bz45DSr7A94VLvY9SWx1GCSZnG
- blowfish :3
  
ralph:147258369
![image 17 15.png](assets/Heal/image%2017%2015.png)
  
going to take-survey.heal.htb/index.php/admin with the creds gives an admin panel
![image 18 14.png](assets/Heal/image%2018%2014.png)
bottom of the page has version number 
![image 19 13.png](assets/Heal/image%2019%2013.png)
  
[https://github.com/N4s1rl1/Limesurvey-6.6.4-RCE/blob/main/exploit.py](https://github.com/N4s1rl1/Limesurvey-6.6.4-RCE/blob/main/exploit.py)
![image 20 10.png](assets/Heal/image%2020%2010.png)
  
/var/www/limesurvey/application/config
![image 21 10.png](assets/Heal/image%2021%2010.png)
  
we love password reuse :3
![image 22 9.png](assets/Heal/image%2022%209.png)
  
forwarding port 8500 gives a new panel
![image 23 8.png](assets/Heal/image%2023%208.png)
  
![image 24 7.png](assets/Heal/image%2024%207.png)
1.19.2
  
[https://www.exploit-db.com/exploits/51117](https://www.exploit-db.com/exploits/51117)
run this exploit with the necessary information
- use [localhost](http://localhost) for rhost and 0 for token
    
    - you can use any value for the token because the database doesn’t even exist