# Company Hosted Servers
can get a good idea of the cloud services they are using
- get subdomainlist from
[[Export-08dd1444-22e8-4c73-ac0f-80d7e9ebe484/CPTS/Footprinting/Enumeration|Enumeration]]
```JavaScript
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```
![image 1 127.png](../assets/Cloud%20Resources/image%201%20127.png)
  
## Google Searches for Cloud
  
### AWS
using inurl: and intext:
```JavaScript
intext:<company> inurl:amazonaws.com
```
![image 2 119.png](../assets/Cloud%20Resources/image%202%20119.png)
  
### Azure
```JavaScript
intext:<company> inurl:blob.core.windows.net
```
![image 3 109.png](../assets/Cloud%20Resources/image%203%20109.png)
  
  
## Target Website - Source Code
source can often have references to cloud services
![image 4 101.png](../assets/Cloud%20Resources/image%204%20101.png)
  
  
# Third-Party Providers
## Domain.Glass
can tell a lot abou company’s infrastructure
![image 5 98.png](../assets/Cloud%20Resources/image%205%2098.png)
  
## GrayHatWarfare
[https://buckets.grayhatwarfare.com/](https://buckets.grayhatwarfare.com/)
can do specific searches, discover AWS, Azure, and GCP cloud storage, and sort and filter by file format
![image 6 88.png](../assets/Cloud%20Resources/image%206%2088.png)
![image 7 85.png](../assets/Cloud%20Resources/image%207%2085.png)