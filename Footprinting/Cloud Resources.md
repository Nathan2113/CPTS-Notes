# Company Hosted Servers
can get a good idea of the cloud services they are using
- get subdomainlist from
[[Export-08dd1444-22e8-4c73-ac0f-80d7e9ebe484/CPTS/Footprinting/Enumeration|Enumeration]]
```JavaScript
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```
![[../assets/Cloud Resources/image 1 127.png|image 1 127.png]]
  
## Google Searches for Cloud
  
### AWS
using inurl: and intext:
```JavaScript
intext:<company> inurl:amazonaws.com
```
![[../assets/Cloud Resources/image 2 119.png|image 2 119.png]]
  
### Azure
```JavaScript
intext:<company> inurl:blob.core.windows.net
```
![[../assets/Cloud Resources/image 3 109.png|image 3 109.png]]
  
  
## Target Website - Source Code
source can often have references to cloud services
![[../assets/Cloud Resources/image 4 101.png|image 4 101.png]]
  
  
# Third-Party Providers
## Domain.Glass
can tell a lot abou company’s infrastructure
![[../assets/Cloud Resources/image 5 98.png|image 5 98.png]]
  
## GrayHatWarfare
[https://buckets.grayhatwarfare.com/](https://buckets.grayhatwarfare.com/)
can do specific searches, discover AWS, Azure, and GCP cloud storage, and sort and filter by file format
![[../assets/Cloud Resources/image 6 88.png|image 6 88.png]]
![[../assets/Cloud Resources/image 7 85.png|image 7 85.png]]