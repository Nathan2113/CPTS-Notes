![image 18.png](/image%2018.png)
  
![image 1 9.png](/image%201%209.png)
login page on https
  
![image 2 10.png](/image%202%2010.png)
lighttpd version 1.4.35
  
vulnerable to CVE-2014-2323
https://github.com/arkede/cve-2014/blob/main/CVE-2014-2323.yaml
  
cve didn’t work, the writeup mentions “system-users.txt” which isn’t in big.txt
- can do the directory-list-medium and it’s in there (reminder to try more than one wordlist if you can’t find anything else
![image 3 9.png](/image%203%209.png)
  
![image 4 7.png](/image%204%207.png)
  
![image 5 6.png](/image%205%206.png)
logged in with the default password pfsense
rohit:pfsense
  
can see that it’s running fpsense 2.1.3-RELEASE
- found a github for the metasploit module
https://github.com/rapid7/metasploit-framework/blob/master/documentation/modules/exploit/unix/http/pfsense_graph_injection_exec.md
  
![image 6 5.png](/image%206%205.png)