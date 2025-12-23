![image 114.png](assets/Access/image%20114.png)
  
  
![image 1 80.png](assets/Access/image%201%2080.png)
  
![image 2 79.png](assets/Access/image%202%2079.png)
  
  
![image 3 72.png](assets/Access/image%203%2072.png)
  
![image 4 68.png](assets/Access/image%204%2068.png)
ftp had backup.mdb (microsoft database) and a password protected zip
- crack zip with zip2john
![image 5 67.png](assets/Access/image%205%2067.png)
- could not crack password
  
to get mdb-tables to work, we have to use an alternative way to download the mdb file since ftp kept breaking after around 14%
- wget --ftp-user=anonymous --ftp-password='' ftp://access.htb/Backups/backup.mdb --no-passive-ftp
    
    - this fully downloaded the file, then the picture below is the result of mdb-tables
    
![image 6 63.png](assets/Access/image%206%2063.png)
  
  
![image 7 61.png](assets/Access/image%207%2061.png)
dumping the auth_users table
  
the engineer password works for the zip file obtained from the ftp server, unzipping that gives a pst file
- pst files are used for storing email info, calendar info, etc and can be read with the readpst tool
  
![image 8 57.png](assets/Access/image%208%2057.png)
![image 9 54.png](assets/Access/image%209%2054.png)
![image 10 46.png](assets/Access/image%2010%2046.png)
![image 11 43.png](assets/Access/image%2011%2043.png)
  
![image 12 36.png](assets/Access/image%2012%2036.png)
![image 13 36.png](assets/Access/image%2013%2036.png)
  
security:4Cc3ssC0ntr0ller
![image 14 33.png](assets/Access/image%2014%2033.png)
  
![image 15 30.png](assets/Access/image%2015%2030.png)
found an SQL password
htrcy@HXeryNJCTRHcnb45CJRY
  
![image 16 27.png](assets/Access/image%2016%2027.png)
Public desktop has a link to ZKAccess3.5 Security System.lnk, which has a privesc from this misconfiguration
[https://www.exploit-db.com/exploits/40323](https://www.exploit-db.com/exploits/40323)
- this didn’t go anywhere
  
in the public desktop there is a lnk pointing to runas.exe
![image 17 22.png](assets/Access/image%2017%2022.png)
  
using the saved creds for runas, we can output root.txt and output it to a file
runas /savecred /user:ACCESS\Administrator "cmd.exe /c type C:\Users\Administrator\Desktop\root.txt > C:\temp\out.txt"out.txt”
  
![image 18 21.png](assets/Access/image%2018%2021.png)