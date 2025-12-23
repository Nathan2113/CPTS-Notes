![image 119.png](assets/Previous/image%20119.png)
  
![image 1 85.png](assets/Previous/image%201%2085.png)
  
![image 2 84.png](assets/Previous/image%202%2084.png)
/api-doc leads to login page
  
[https://nvd.nist.gov/vuln/detail/CVE-2025-29927](https://nvd.nist.gov/vuln/detail/CVE-2025-29927)
- CVE for Next.js 15.2.2
  
according to a writeup, you need to hit a directory that requires authentication in order for the auth bypass to work
- they found that there was a directory /api/download
![image 3 77.png](assets/Previous/image%203%2077.png)
- next they did a fuzz checking all parameters for the correct parameter to “download”
![image 4 73.png](assets/Previous/image%204%2073.png)
- since we know that the parameter is “example”, we can send a new request and get LFI using the modified header
```JavaScript
curl 'http://previous.htb/api/download?example=../../../../etc/passwd' -H 'X-Middleware-Subrequest: middleware:middleware:middleware:middleware:middleware'
```
- since we now have LFI, we can search for creds that way
  
trying the exploit from before with this new information worked
![image 5 72.png](assets/Previous/image%205%2072.png)
  
when looking for environmental variables, you can also just do /proc/self/environ
- can be useful if you don’t know where .env is
  
doing some research into the directory trees for next.js, we can find a path to exploit
![image 6 68.png](assets/Previous/image%206%2068.png)
.next is the output for the application
  
in the .next directory, there is a routes.manifest that shows us some of the routing logic
![image 7 65.png](assets/Previous/image%207%2065.png)
  
in the dynamic routes, it shows us where the authentication logic is
![image 8 61.png](assets/Previous/image%208%2061.png)
  
curling that page (with URL encoding), gives us user credentials
![image 9 58.png](assets/Previous/image%209%2058.png)
  
![image 10 49.png](assets/Previous/image%2010%2049.png)
  
jeremy can run terraform as root
![image 11 46.png](assets/Previous/image%2011%2046.png)
  
jeremy:MyNameIsJeremyAndILovePancakes
  
can run terraform as root, but only in the /opt/examples directory
- sudo /usr/bin/terraform -chdir\=/opt/examples apply
  
![image 12 39.png](assets/Previous/image%2012%2039.png)
terraform is pulling binaries from the root terraform through a symlink
  
with terraform, if we are able to change which configuration files are we can privesc
- create a provider file that changes the suid bit on /bin/bash
![image 13 39.png](assets/Previous/image%2013%2039.png)
  
now create the configuration file that will set the new provider to the malicious bash script
![image 14 36.png](assets/Previous/image%2014%2036.png)
  
run terraform apply
![image 15 33.png](assets/Previous/image%2015%2033.png)
  
do “bash -p” to change to root
![image 16 29.png](assets/Previous/image%2016%2029.png)
  
this box is kinda insane