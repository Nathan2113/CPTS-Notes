![image 122.png](assets/Armageddon/image%20122.png)
  
![image 1 88.png](assets/Armageddon/image%201%2088.png)
  
  
  
![image 2 87.png](assets/Armageddon/image%202%2087.png)
  
![image 3 80.png](assets/Armageddon/image%203%2080.png)
  
![image 4 76.png](assets/Armageddon/image%204%2076.png)
/modules/user/user.info has the exact drupal version number
- 7.56
![image 5 75.png](assets/Armageddon/image%205%2075.png)
found exploitdb page with correct version number
- CVE-2018-7600
  
https://github.com/firefart/CVE-2018-7600
![image 6 71.png](assets/Armageddon/image%206%2071.png)
- was able to get some info
  
couldn’t get ruby version of script to work, so i used metasploit
![image 7 68.png](assets/Armageddon/image%207%2068.png)
  
![image 8 64.png](assets/Armageddon/image%208%2064.png)
4S4JNzmn8lq4rqErTvcFlV4irAJoNqUmYy_d24JEyns
  
![image 9 61.png](assets/Armageddon/image%209%2061.png)
CQHEy@9M*m23gBVj
  
![image 10 52.png](assets/Armageddon/image%2010%2052.png)
gitignore shows some places to look
- already saw sql creds in settings.php, but when trying to use mysql it doesn’t work with our shell
- nothing good
  
![image 11 49.png](assets/Armageddon/image%2011%2049.png)
mysql is authenticating with drupaluser, but for some reason commands don’t output to anything
  
we can have mysql run a single command and output that result to a file
![image 12 42.png](assets/Armageddon/image%2012%2042.png)
![image 13 42.png](assets/Armageddon/image%2013%2042.png)
bruce’s hash
- $S$DgL2gjv6ZtxBo6CdqZEyJuBphBmrCqIV6W97.oOsUf1xAhaadURt
![image 14 39.png](assets/Armageddon/image%2014%2039.png)
![image 15 36.png](assets/Armageddon/image%2015%2036.png)
  
![image 16 32.png](assets/Armageddon/image%2016%2032.png)
  
![image 17 26.png](assets/Armageddon/image%2017%2026.png)
  
[https://gtfobins.github.io/gtfobins/snap/](https://gtfobins.github.io/gtfobins/snap/)
![image 18 24.png](assets/Armageddon/image%2018%2024.png)
  
do the top box in our own machine, upload it to the victim computer, and run the sudo snap install command
![image 19 22.png](assets/Armageddon/image%2019%2022.png)
![image 20 19.png](assets/Armageddon/image%2020%2019.png)
![image 21 19.png](assets/Armageddon/image%2021%2019.png)
![image 22 16.png](assets/Armageddon/image%2022%2016.png)